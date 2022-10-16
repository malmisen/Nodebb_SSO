#Installing SSO with google#
https://github.com/julianlam/nodebb-plugin-sso-google

## 1. Install the plugin on Nodebb (nodebb-plugin-sso-google) ##
	Either do this via Dashboard or npm
	1. via the admin Dashboard on Nodebb
	2. npm install nodebb-plugin-sso-google


## 2. Setting it up ##
 1. API Console - https://console.cloud.google.com/projectselector2/apis/dashboard?supportedpurview=project
	- Create a New Project via the API Console
	- Lets call this new project "nodebb"
	
 2. From the "Credentials" page, create a new "OAuth Client ID"
		1. click on "+ CREATE CREDENTIALS"
		2. OAuth client ID
		3. Complete the OAuth consent setup (I picked External user type)
		4. Fill in App information
			- Nodebb
			- <email>
			- App logo whatever
			- application home page = http://localhost:4567
			- Developer contact information = <email>
			- I leave scope, rate limits and such things as default since this is only for illustration purposes
			- Add a user under "Test users"
				-  <email>
		
		5. Click on "+ CREATE CREDENTIALS ONCE MORE
			- Application Type - Web application
			- Name - Nodebb
			- Authorized JavaScript origins - http://localhost:4567/
			- Authorized redirect URIs - http://localhost:4567/auth/google/callback
			- Save the clientId and clientSecret, you will need them when configuring
			  the plugin on NodeBB
	
		clientId: <clientId>
		clientSecret: <clientSecret>

## 3. Configure plugin ##
	1. install plugin
	2. activate plugin
	3. rebuild - restart nodebb
	4. Click admin dashboard -> plugins -> social authentication google 
	5. Insert the client id and secret you generated 
	6. Save and restart and rebuild
		./nodebb stop
		./nodebb build
		./nodebb start

  #### NOTE! NodeBB does not appreciate special characters! Do not use any when creating your google account. ####
	1. Create a google account
		- firstname
		- lastname
		- <accName>
		- <password>

You should now be able to register on NodeBB with your google account.
Then you can login with your newly registered account.
