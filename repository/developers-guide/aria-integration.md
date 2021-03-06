# ARIA integration

Aria integration follows docs at [http://aria.structuralbiology.eu/docs.html ](http://aria.structuralbiology.eu/docs.html)

{% hint style="info" %}
with slight modification explained in this type of info box
{% endhint %}

The following steps needs to be performed in order to get proposals from ARIA database: 1. your application needs first generate special link with encrypted client\_id and secret\_id 2. when user clicks the link, first it is redirected to Instuct site to authenticate and authorize access to the Repository web app. When redirected back, it is redirected with access code and state in url parameters. 3. with access code, web application requests access token. 4. with access token, web application requests list of proposals 5. user can initiate to request proposal detail, it is obtained using access token and proposal ID \(pid\).

## Step 1 Obtain link

Your application is hosted at [http://\[yourweb\]/index.html](http://[yourweb]/index.html). And contains e.g. following link which should initiate importing data on user request.

`index.html`:

```markup
...
<a href="">Import data</a>
...
```

The link is empty and needs to be filled, e.g. using the Fetch API: `ariaapi.js`:

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

In order not to reveal `client_id`and `client_secret`, the following code reside on server/backend side only of your application at http:\[yourweb\]/accessToken.php:

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

This returns link which can be bind to the HTML by your preffered Javascript framework \(Aurelia,Angular,React,Jquery,clean Javascript using DOM api only\). So the link is now prepared `index.html`:

```markup
...
<a href="http://structuralbiology.eu/authorize?client_id=123&state=456&response_type=code">Import data</a>
...
```

## Step 2 Obtain Access Code

When the link is clicked by the user, it redirects to Instruct site and user is asked to authorize access to his data to your aplication at \[yourweb\]. If succesfull, Instruct site will redirect user's browser back to your web using parameters in URL, e.g. [http://\[yourweb\]/index.html?code=789&state=012](http://[yourweb]/index.html?code=789&state=012) Therefore your web application needs to detect parameters in url, e.g. by following code `index.js`

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

## Step 3 Obtain Access Token

The parameters `code` and `state` are used to obtain access token from ARIA proxied by the `accessToken.php` service `ariaapi.js`

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

{% hint style="info" %}
PHP7 does not allow credentials in the body of a request. Thus client id and secret is sent as HTTP headers `curl_setopt( $ch, CURLOPT_USERPWD, $client_id .':'. $client_secret );`.
{% endhint %}

`accessToken.php part 2`

```php
...
if(isset($_GET['code']) && isset($_GET['state'])){
...
  // manually build OAuth packet - this can be replaced for an off-shelf solution
  $OApacket = array(
    'grant_type' => 'authorization_code',
    'code' => urlencode($code)
  );
  // start curl connection
  $ch = curl_init();
  ...
  // add OAuth packet and define HTTP POST
  curl_setopt( $ch, CURLOPT_POST, true );
  curl_setopt( $ch, CURLOPT_POSTFIELDS, http_build_query($OApacket) );
  curl_setopt( $ch, CURLOPT_USERPWD, $client_id .':'. $client_secret );
  // run curl and capture output
  if(!$content = curl_exec( $ch )) { ...
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

## Step 4 Get Proposal list

As the web app now has the access\_token, it can use it to request ARIA directly \(not via accessToken.php\). To get user's proposallist and proposal details: `index.js`

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

{% hint style="info" %}
Note, the HTTP GET request is used to obtain the proposallist
{% endhint %}

## Step 5 Get Proposal list

`index.js`

```javascript
 this.ariaapi.getProposal(pid,accesstoken).then(list =>{ //show list})
```

`ariaapi.js`

```javascript
 ...
   getProposal(pid,accesstoken) {
      return this.httpclient.fetch("https://www.structuralbiology.eu/ws/oauth/proposal?pid="+ pid+"&access_token="
      +accesstoken.access_token)
        .then(response => response.json())
        .then(data => { return data })
      }
...
```

{% hint style="info" %}
Note, the HTTP GET request is used to obtain the proposal detail
{% endhint %}

## Full working code

Working sample code is in [github.com/h2020-westlife-eu/wp6-repository/tree/master/frontend](https://github.com/h2020-westlife-eu/wp6-repository/tree/master/frontend)

