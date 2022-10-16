Two way authentication

1) As admin locate ACP plugins in Nodebb
2) Check Inactivte plugins
3) find nodebb-plugin-2factor and install it
4) Once installed, run ./nodebb setup 
5) Click on your profile, locate the drop down menu in your profile and click on two-factor authentication
6) Enable the desired setup method for the two factor authentication challenge.
(Here we picked Authenticator App and went with Google Authentication)
 - We download the Google Authenticator App mobile device
 - Scan the QR-code with the App
 - Insert the 6 digit pin
7) Two way auth is now setup!

Single Sign on Nodebb

Database installation for FusionAuth
	postgresql (for macOS)
		1) Navigate to https://postgresapp.com/
		2) click on the download tab and download the latest version of PostgreSQL
		3) Once the download is complete click on "Intialize" in the GUI to start the server
		4) Head to https://postgresapp.com/ once more and click on "Introduction"
		5) Configure your $PATH to use the included command line tools
			sudo mkdir -p /etc/paths.d && echo /Applications/Postgres.app/Contents/Versions/latest/bin | sudo tee /etc/paths.d/postgresapp
		6) Make sure it works by typing psql --version
		7) Done!

FusionAuth
	Installing FusionAuth (for macOS)	
		1) Test Node and psql installtion
			node -v and psql --version
		2) Install in your current working directory using ZIP packages
			sh -c "curl -fsSL https://raw.githubusercontent.com/FusionAuth/fusionauth-install/master/install.sh | sh"
		3) To start FusionAuth run the following command
			/Users/bpontarelli/dev/example/fusionauth/bin/startup.sh
			or
			fusionauth/bin/startup.sh
		4) Access FusionAuth by opening a browser to http://localhost:9011
		5) Complete Maintenance Mode
			FusionAuth Credentials:
				acc: <accountName>
				pass: <password>
		6) Complete the Setup Wizard	
			acc: <email>	 
			pass: <password>

	Create an Application and configure the OAuth settings
		1) Click On "Applications"
		2) Select "Setup" in the missing application box
		3) Click on the green+ button to create a new application
		4) We give our new application the name nodebb
		5) Save the client Id and the client secret for later
			client id: 	<someId>
			secret: 	<someSecret>
		6) Set 
			Authorized redirect url to: http://localhost:4567/auth/fusionauth-oidc/callback
			Authorized request origin URLs to: http://localhost:4567
			Logout URL to: http://localhost:4567/
	Add a user in FusionAuth
		1) click on "users" and create a new user 
			acc:  fornodebb@example.com
			pass: hejhejhej
		2) Grant the user permissions to your application 
		3) Click on users once more and locate the "Manage" button for the created user
		4) When inside "Manage User" click on "Add registration"
		5) Select nodebb as application

NodeBB (for macOS)(We downgrade version of nodebb to 1.19.x for compatibility)
	First we install a database
		1) brew install redis
		2) redis-server

	Then we install NodeBB
		1) git clone -b v1.19.x https://github.com/NodeBB/NodeBB.git
		2) cd NodeBB
		3) ./nodebb setup
		4) ./nodebb start
	
	In the terminal install nodebb-plugin-fusionauth-oidc
		1) npm install nodebb-plugin-fusionauth-oidc
	Login as administrator 
		1) Find the Admin Dashboard
		2) Click on Plugins
		3) Select OpenID Connect
		4) fill in the form:
			client id: 			<someId>
			secret: 			<someSecret>
			Discovery URL:		http://localhost:4567/
			Authorization endpoint:	http://localhost:4567/auth/fusionauth-oidc/callback
