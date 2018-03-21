# ARIA integration

Aria integration follows docs at [http://aria.structuralbiology.eu/docs.html ](http://aria.structuralbiology.eu/docs.html)with these modification:

1. your application needs to obtain access code - in order to do that your application authenticates using client\__\_id and secret\_id and randomly generated salt._
2. with access code a user within your application can ask for authorization token
3. with authorization token user can raise calls to obtain his data within Instruct database - currently proposal for research visit in Instruct site.

```uml

@startuml
skinparam sequenceArrowThickness 1
skinparam roundcorner 10
skinparam maxmessagesize 60

actor User
box "Your web app at http://[yourweb]" #LightBlue
  participant "index.html" as index
  participant "accessToken.php" as php
end box
box "ARIA Api at https://structuralbiology.eu/" #LightGreen
participant "authorize" as authorize
participant "oauth" as oauth
participant "oauth/proposallist" as list
participant "oauth/proposal" as proposal
end box
User -> index: get
activate index

index -> php: generate state
activate php

php --> index: state

index --> User: link with state

User -> index: click link
index -> authorize: get access_code
activate authorize
authorize --> User: redirect
deactivate index
User -> authorize: confirm 
authorize --> index: redirect
deactivate authorize
activate index
index --> User: access_code
User -> index: click on import
index --> php: code and state
php -> oauth: code and state
activate oauth
oauth --> php: access token
deactivate php
deactivate oauth
php --> index: access token
index -> list: get(access_token)
activate list
list --> index: list
deactivate list
index --> User: show list
User -> index: click on proposal
index --> proposal: get(pid,access_token)
activate proposal
proposal --> index: detail
deactivate proposal
index --> User: show detail

@enduml
```

## Step 1 Obtain access code

Your application is hosted at http://[yourweb]/index.html. And contains e.g. following link which should initiate importing data on user request.

`index.html`:
```html
...
<a href="">Import data</a>
...
```
The link is empty and needs to be filled, e.g. using the Fetch API:
`ariaapi.js`:
```javascript
    ...
    return this.httpclient.fetch('/accessToken.php')
    //expecting JSON with {'url':'http://structuralbiology.eu/
    //   authorize?client_id=123&state=456&response_type=code'}
      .then(response => response.json())
      .then(data => {
        return // use data.url to add it into <a href="${data.url}">get access token</a>
      })
      ...
```
In order not to reveal client_id and client_secret, the following code reside on server/backend side of your application at http:[yourweb]/accessToken.php: 

`accessToken.php part 1`:

```php
<?php
//$client_id = 'abc123...';
//$client_secret = 'ABC123...';
$headerHost = $_SERVER['HTTP_HOST'];
$OAservice = 'https://www.structuralbiology.eu/ws/oauth/';
// define a unique state string for security
session_start();
if(!isset($_SESSION['OAstate'])) {
  $_SESSION['OAstate'] = rand(1000000, 9999999);
}
error_log("session state:".$_SESSION['OAstate']);
// if we have code, then retrieve token and set it to cookie
if(isset($_GET['code']) && isset($_GET['state'])){
  ...
}
//otherwise create link
else {
  header("Status: 200 OK");
  header('Content-type: application/json');
  echo '{"url":"'.$OAservice.'authorize?client_id='.$client_id.'&state='.$_SESSION['OAstate'].'&response_type=code"}';
  exit;
}
?>
```
This returns link which can be bind to the HTML by your preffered Javascript framework (Aurelia,Angular,React,Jquery,clean Javascript using DOM api only). So the link is now prepared 
`index.html`:
```html
...
<a href="http://structuralbiology.eu/authorize?client_id=123&state=456&response_type=code">Import data</a>
...
```
## Step 2 Obtain Access Token
When the link is clicked by the user, it redirects to Instruct site and user is asked to authorize access to his data to your aplication at [yourweb]. If succesfull, Instruct site will redirect user's browser back to your web using some params in URL, e.g. http://[yourweb]/index.html?code=789&state=012
Therefore your web application needs detect this state by following snippet
`index.js`
```javascript
//
const getParams = query => {
  if (!query) {
    return {};
  }

  return (/^[?#]/.test(query) ? query.slice(1) : query)
    .split('&')
    .reduce((params, param) => {
      let [key, value] = param.split('=');
      params[key] = value ? decodeURIComponent(value.replace(/\+/g, ' ')) : '';
      return params;
    }, {});
};
...
    this.params=getParams(window.location.search.substring(1));
    if (this.params.code && this.params.state)
      this.ariaapi.getAccessToken(this.params.code,this.params.state);
```
Then the parameters `code` and `state` are used to obtain access token from ARIA proxied by the `accessToken.php` service 
`ariaapi.js`
```javascript
  getAccessToken(code,state){
    return this.httpclient.fetch("/accessToken.php?code="+code+"&state="+state)
      .then(data => {
        this.accesstoken=JSON.parse(data.response);
        return this.accesstoken;
      })
      ...
  }
```
`accessToken.php part 2`
```php
...
if(isset($_GET['code']) && isset($_GET['state'])){
  $code = $_GET['code'];
  $state = $_GET['state'];

  // quickly sanitize input
  if(!preg_match('/[A-Za-z0-9]+/', $code))
    exit('Authentication code invalid');
  if(!preg_match('/[A-Za-z0-9]+/', $state))
    exit('State is invalid');
  // verify the state before continuing on
  if($state != $_SESSION['OAstate'])
    exit('State is invalid: state:'.$state.' oastate:'.$_SESSION['OAstate']);
  // manually build OAuth packet - this can be replaced for an off-shelf solution
  $OApacket = array(
    'grant_type' => 'authorization_code',
    'code' => urlencode($code)
  );
  // start curl connection
  $ch = curl_init();
  curl_setopt( $ch, CURLOPT_URL, $OAservice.'token' );
  curl_setopt( $ch, CURLOPT_FOLLOWLOCATION, true );
  curl_setopt( $ch, CURLOPT_RETURNTRANSFER, true );
  curl_setopt( $ch, CURLOPT_AUTOREFERER, true );
  //TODO VERIFYPEER,true fails on SL7
  curl_setopt( $ch, CURLOPT_SSL_VERIFYPEER, false );
  curl_setopt( $ch, CURLOPT_SSL_VERIFYHOST, 0 );
  curl_setopt( $ch, CURLOPT_CAINFO, "cacert.pem" ) ;
  curl_setopt( $ch, CURLOPT_MAXREDIRS, 10 );
  // add OAuth packet and define HTTP POST
  curl_setopt( $ch, CURLOPT_POST, true );
  curl_setopt( $ch, CURLOPT_POSTFIELDS, http_build_query($OApacket) );
  curl_setopt( $ch, CURLOPT_USERPWD, $client_id .':'. $client_secret );
  // run curl and capture output
  if(!$content = curl_exec( $ch )) {
    error_log("error occured errno:".curl_errno($ch));
    error_log(curl_error($ch));
    header("Status: 404 Not Found"); //TODO return HTTP 404 403
    echo('error:'.curl_error($ch));
    exit('Could not fetch access token');
  }
  curl_close ( $ch );
  // convert JSON into object
  if(!$authorization = json_decode($content))
    exit('JSON parsing error');
  if($authorization->access_token){
    // we will store access tokens as cookies for this demo
    setcookie('access_token', $authorization->access_token, time()+$authorization->expires_in);
    // this refresh token actually has an expiry of 28 days, however for the demo it's useful to expire within a single day
    setcookie('refresh_token', $authorization->refresh_token, time()+60*60*24*1);
    $_COOKIE['access_token'] = $authorization->access_token;
    //now exit - cookie set - no other content is needed
    header("Status: 200 OK");
    header('Content-type: application/json');
    echo ($content);
  } else {
    header("Status: 404 Not Found");
    header('Content-type: application/json');
    echo ($content);
  }
  exit;
...
?>
```
## Step 3 Get Data
As the web app now has the access_token, it can use it to request ARIA directly (not via accessToken.php) to get user's proposallist and proposal details:
`index.js`
```javascript
 this.ariaapi.getProposalList(accesstoken).then(list =>{ //show list})
 ```
 `ariaapi.js`
 ```javascript
 ...
   getProposalList(accesstoken) {
      return this.httpclient.fetch("https://www.structuralbiology.eu/ws/oauth/proposallist?access_token="
      +this.accesstoken.access_token)
        .then(response => response.json())
        .then(data => { return data })
      }
...

```

## Full working code
Working sample code is in 
