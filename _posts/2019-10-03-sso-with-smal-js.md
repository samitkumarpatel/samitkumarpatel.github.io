-----

-----

**Secure Static Webpage with msal.js**

An Example 

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <title>vue-msal-poc</title>
  </head>
  <body>
    <noscript>
      <strong>We're sorry but vue-msal-poc doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
    <div id="app"></div>

    <h2>Welcome to MSAL.js Quickstart</h2><br/>
    <h4 id="WelcomeMessage"></h4>
    <button id="SignIn" onclick="signIn()">Sign In</button><br/><br/>
    <pre id="json"></pre>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
    <script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.1.3/js/msal.js"></script>
    <script>
      var msalConfig = {
          auth: {
              clientId: "CLIENT_ID",
              authority: "https://login.microsoftonline.com/TENANT_ID"
          },
          cache: {
              cacheLocation: "localStorage",
              storeAuthStateInCookie: true
          }
      };
      
      var graphConfig = {
          graphMeEndpoint: "https://graph.microsoft.com/v1.0/me"
      };
      
      // this can be used for login or token request, however in more complex situations
      // this can have diverging options
      var requestObj = {
          scopes: ["user.read"]
      };
      
      var myMSALObj = new Msal.UserAgentApplication(msalConfig);
      // Register Callbacks for redirect flow
      myMSALObj.handleRedirectCallback(authRedirectCallBack);
      
      
      function signIn() {
      
          myMSALObj.loginPopup(requestObj).then(function (loginResponse) {
              //Login Success
              showWelcomeMessage();
              acquireTokenPopupAndCallMSGraph();
          }).catch(function (error) {
              console.log(error);
          });
      }
      
      function signOut() {
        myMSALObj.logout();
      }

      function acquireTokenPopupAndCallMSGraph() {
          //Always start with acquireTokenSilent to obtain a token in the signed in user from cache
          myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
              callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
          }).catch(function (error) {
              console.log(error);
              // Upon acquireTokenSilent failure (due to consent or interaction or login required ONLY)
              // Call acquireTokenPopup(popup window)
              if (requiresInteraction(error.errorCode)) {
                  myMSALObj.acquireTokenPopup(requestObj).then(function (tokenResponse) {
                      callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
                  }).catch(function (error) {
                      console.log(error);
                  });
              }
          });
      }
      
      
      function graphAPICallback(data) {
          document.getElementById("json").innerHTML = JSON.stringify(data, null, 2);
      }
      
      
      function showWelcomeMessage() {
          var divWelcome = document.getElementById('WelcomeMessage');
          divWelcome.innerHTML = 'Welcome ' + myMSALObj.getAccount().userName + "to Microsoft Graph API";
          var loginbutton = document.getElementById('SignIn');
          loginbutton.innerHTML = 'Sign Out';
          loginbutton.setAttribute('onclick', 'signOut();');
      }
      
      
      //This function can be removed if you do not need to support IE
      function acquireTokenRedirectAndCallMSGraph() {
          //Always start with acquireTokenSilent to obtain a token in the signed in user from cache
          myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
              callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
          }).catch(function (error) {
              console.log(error);
              // Upon acquireTokenSilent failure (due to consent or interaction or login required ONLY)
              // Call acquireTokenRedirect
              if (requiresInteraction(error.errorCode)) {
                  myMSALObj.acquireTokenRedirect(requestObj);
              }
          });
      }
      
      
      function authRedirectCallBack(error, response) {
          if (error) {
              console.log(error);
          }
          else {
              if (response.tokenType === "access_token") {
                  callMSGraph(graphConfig.graphEndpoint, response.accessToken, graphAPICallback);
              } else {
                  console.log("token type is:" + response.tokenType);
              }
          }
      }
      
      function requiresInteraction(errorCode) {
          if (!errorCode || !errorCode.length) {
              return false;
          }
          return errorCode === "consent_required" ||
              errorCode === "interaction_required" ||
              errorCode === "login_required";
      }
      
      // Browser check variables
      var ua = window.navigator.userAgent;
      var msie = ua.indexOf('MSIE ');
      var msie11 = ua.indexOf('Trident/');
      var msedge = ua.indexOf('Edge/');
      var isIE = msie > 0 || msie11 > 0;
      var isEdge = msedge > 0;
      //If you support IE, our recommendation is that you sign-in using Redirect APIs
      //If you as a developer are testing using Edge InPrivate mode, please add "isEdge" to the if check
      // can change this to default an experience outside browser use
      var loginType = isIE ? "REDIRECT" : "POPUP";
      
      if (loginType === 'POPUP') {
          if (myMSALObj.getAccount()) {// avoid duplicate code execution on page load in case of iframe and popup window.
              showWelcomeMessage();
              acquireTokenPopupAndCallMSGraph();
          }
      }
      else if (loginType === 'REDIRECT') {
          document.getElementById("SignIn").onclick = function () {
              myMSALObj.loginRedirect(requestObj);
          };
          if (myMSALObj.getAccount() && !myMSALObj.isCallback(window.location.hash)) {// avoid duplicate code execution on page load in case of iframe and popup window.
              showWelcomeMessage();
              acquireTokenRedirectAndCallMSGraph();
          }
      } else {
          console.error('Please set a valid login type');
      }
    </script>
  </body>
</html>


```
