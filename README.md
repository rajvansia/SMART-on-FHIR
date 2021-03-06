## SMART on FHIR launch flow sample app for learning purposes.
#Now hosted at https://smart-test-app.herokuapp.com/
#specify the launch endpoint in any EMR launch tester as https://smart-test-app.herokuapp.com/launch
Note - if you launch from the herokuapp site, you do not need to perform the below setup.  You're calling my hosted version which probably won't have a client ID/secret available for the EMR you're working with.

# SMART-on-FHIR
1. Get a client ID and secret and set app as confidential with EMR vendor
2. Put your client ID and secret into a client.json file in the root folder.
3. Set your launch URI to http://localhost:3000/launch
4. Set your redirect URI to http://localhost:3000/code
5. npm install
6. npm start
7. Hit the buttons as they show up and see what is getting passed around during the OAuth2 workflow.

Format of client.json:
{
    "clientId" : "client ID",
    "clientSecret" : "client secret"
}

# OAuth2 flow

## Express server output:
* GET /launch?iss=https%3A%2F%2Fsb-fhir-dstu2.smarthealthit.org%2Fsmartdstu2%2Fdata&launch=Qk9g9o 200 1123.533 ms - 748
* POST /auth 302 39.403 ms - 642
* GET /code?code=SAs65J&state=123 200 32.805 ms - 304
* GET /access?code=SAs65J&state=123 200 409.273 ms - 823

## Steps:
1. Launch via EMR/sandbox
    * Land on the /launch page.
    * Query string parameter for iss will be FHIR base URL
    * Query string parameter for launch will be launch token
2. Hit button to exchange launch token for authorization code
    * GET to iss/metadata endpoint to get OAuth auth/token URLs.
    * POST to /launch/auth endpoint with launch token to get authorization code.
    * Redirect will land you on /launch/code page.
        * Access code will be in the code query string parameter.
3. Hit button to get access token
    * GET to /launch/access endpoint will in turn POST to token endpoint.
        * Authorization header using Basic auth + base64 encoded combination of clientID:clientSecret.
4. Land on /launch/access page with access token on page.
