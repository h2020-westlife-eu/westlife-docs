# ARIA integration

Aria integration follows docs at [http://aria.structuralbiology.eu/docs.html ](http://aria.structuralbiology.eu/docs.html)with these modification:

1. step - your application needs to obtain access code - in order to do that your application authenticates using client\__\_id and secret\_id and randomly generated salt._
2. step - with access code a user within your application can ask for authorization token
3. step - with authorization token user can raise calls to obtain his data within Instruct database - currently proposal for research visit in Instruct site





