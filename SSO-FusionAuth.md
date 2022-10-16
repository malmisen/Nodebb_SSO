# Single Sign on Nodebb #

## 1. Database installation for FusionAuth, postgresql (for macOS)##
		1. Navigate to https://postgresapp.com/
		2. click on the download tab and download the latest version of PostgreSQL
		3. Once the download is complete click on "Intialize" in the GUI to start the server
		4. Head to https://postgresapp.com/ once more and click on "Introduction"
		5. Configure your $PATH to use the included command line tools
			sudo mkdir -p /etc/paths.d && echo /Applications/Postgres.app/Contents/Versions/latest/bin | sudo tee /etc/paths.d/postgresapp
		6. Make sure it works by typing psql --version
		7. Done!

## 2. FusionAuth, Installing FusionAuth (for macOS) ##
	1. Installation
		1. Test Node and psql installtion
			- node -v and psql --version
		2. Install in your current working directory using ZIP packages
			- sh -c "curl -fsSL https://raw.githubusercontent.com/FusionAuth/fusionauth-install/master/install.sh | sh"
		3. To start FusionAuth run the following command
			- /Users/bpontarelli/dev/example/fusionauth/bin/startup.sh
			- or
			- fusionauth/bin/startup.sh
		4. Access FusionAuth by opening a browser to http://localhost:9011
		5. Complete Maintenance Mode
			- FusionAuth Credentials:
				- acc: <accountName>
				- pass: <password>
		6. Complete the Setup Wizard	
			- acc: <email>	 
			- pass: <password>

	2. Create an Application and configure the OAuth settings
		1. Click On "Applications"
		2. Select "Setup" in the missing application box
		3. Click on the green+ button to create a new application
		4. We give our new application the name nodebb
		5. Save the client Id and the client secret for later
			- client id: 	<someId>
			- secret: 	<someSecret>
		6. Set 
			- Authorized redirect url to: http://localhost:4567/auth/fusionauth-oidc/callback
			- Authorized request origin URLs to: http://localhost:4567
			- Logout URL to: http://localhost:4567/
	3. Add a user in FusionAuth
		1. click on "users" and create a new user 
			- acc:  fornodebb@example.com
			- pass: hejhejhej
		2. Grant the user permissions to your application 
		3. Click on users once more and locate the "Manage" button for the created user
		4. When inside "Manage User" click on "Add registration"
		5. Select nodebb as application

## 3. NodeBB (for macOS)(We downgrade version of nodebb to 1.19.x for compatibility) ##
	1. First we install a database
		- brew install redis
		- redis-server

	2. Then we install NodeBB
		- git clone -b v1.19.x https://github.com/NodeBB/NodeBB.git
		- cd NodeBB
		- ./nodebb setup
		- ./nodebb start
	
	3. In the terminal install nodebb-plugin-fusionauth-oidc
		- npm install nodebb-plugin-fusionauth-oidc
	4. Login as administrator 
		1. Find the Admin Dashboard
		2. Click on Plugins
		3. Select OpenID Connect
		4. fill in the form:
			- client id: 			<someId>
			- secret: 			<someSecret>
			- Discovery URL:		http://localhost:4567/
			- Authorization endpoint:	http://localhost:4567/auth/fusionauth-oidc/callback
