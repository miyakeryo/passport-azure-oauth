# Passport-Azure-OAuth

[Passport](http://passportjs.org/) strategy for authenticating with [Azure](https://login.windows.net/common/oauth2) OAuth 2.0 API.

This module lets you authenticate using Azure in your Node.js applications.
By plugging into Passport, Azure / Office 365 authentication can be easily and unobtrusively integrated into any application or framework that supports [Connect](http://www.senchalabs.org/connect/)-style middleware, including [Express](http://expressjs.com/).

## Installation

    $ npm install passport-azure-oauth

## Usage

#### Configure Strategy

The Azure authentication strategy authenticates users using a Azure / Microsoft Office 365
account using OAuth 2.0.  The strategy requires a `verify` callback, which
accepts these credentials and calls `done` providing a user, as well as
`options` specifying a app ID, app secret, and callback URL.

    passport.use(new AzureOAuthStrategy({
        clientId	: AzureOAuth_ClientId,
    	clientSecret: AzureOAuth_ClientSecret,
		tenantId 	: AzureOAuth_AppTenantId,
		resource 	: AzureOAuth_AuthResource
      },
      function(accessToken, refreshToken, profile, done) {
      	return done(err, user);
      }
    ));

* clientId : Id of the registered azure online application.
* clientSecret : Password of the registered azure online application.
* tenantId : Open Azure Online, navigate to the application, click on "VIEW ENDPOINTS", copy the GUID after the host url.
* resource : Url to the Azure / Office 365 resource your app wants to access.
	* e.g.: "https://outlook.office365.com/" to access Office 365 Mail Api
* callbackURL : The redirect url after the authentication.

#### Authenticate Requests

Use `passport.authenticate()`, specifying the `'azureOAuth'` strategy, to
authenticate requests.

For example, as route middleware in an [Express](http://expressjs.com/)
application:

    app.get('/auth/azureOAuth',
      passport.authenticate('azureOAuth'),
      function(req, res){
        // The request will be redirected to SharePoint for authentication, so
        // this function will not be called.
      });

    app.get('/auth/azureOAuth/callback', 
      passport.authenticate('azureOAuth', { 
		failureRedirect: '/login',
		refreshToken: azureOAuth_RefreshToken 
	  }),
      function(req, res) {
        // Successful authentication, redirect home.
        res.redirect('/');
      });

* refreshToken : After one initial authentication, you get a refresh token. You can pass it to the authenticate method to simply renew your access token without a new callback.
## Credits

  - [QuePort](https://github.com/QuePort)
  - [Thomas Herbst](https://github.com/macrauder)
  - [Tobias Winkler](https://github.com/Tschuck)

## License

(The MIT License)

Copyright (c) 2013 Thomas Herbst / QuePort

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.