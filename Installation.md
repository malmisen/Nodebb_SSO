# Step 1: Install Node.js #

(Installing from the NodeSource rep)
	code($ apt-get update)
	code($ apt-get upgrade)
install the Node.js
	$ apt-get install curl git gcc g++ make
	$ curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
	$ apt-get install nodejs
	
Step 2: Install MongoDB

MongoDB is the default database for NodeBB. 
Start the installation by importing the public key used by the package management system.
	$ apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
Add the MongoDB repository:
	$ echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb.list
Update the apt package index and install the MongoDB server:
	$ apt-get update
	$ apt-get install -y mongodb-org
Start the MongoDB service:
	$ systemctl start mongod.service
	$ systemctl enable mongod.service

Step 3: Configure MongoDB

Log in to MongoDB running the following commands:
	$ mongo
Then switch the db to ‘admin’ and create a new admin user…
	use admin
Create a new admin user named ‘admin’ with a new password…
	db.createUser({ user: "admin", pwd: "admin123", roles: [ { role: "readWriteAnyDatabase", db: "admin" }, { role: "userAdminAnyDatabase", db: "admin" }]})
Next, create a new database called nodebb
	use nodebb
Then create a new NodeBB user named ‘nodebbuser’ with rights to administer the database…
	db.createUser({user:"nodebbuser",pwd:"nodebb123",roles:[{role:"readWrite",db:"nodebb"},{role:"clusterMonitor",db:"admin"}]})
Note (Might be smart to replace pwd: "admin123" and "nodebb123"  to something else)
After that, exit the MongoDB shell.
	quit()
After that, run the commands below to open MongoDB config file…
	$ sudo vim /etc/mongod.conf
	#security
	security:
	authorization:enabled

Step 4: Install Nginx
	$ sudo apt-get install nginx
	$ sudo systemctl start nginx.service
	$ sudo systemctl enable nginx.service

Step 5: Installing NodeBB
Go to the newly created directory by executing:
	$ cd /var/www
Clone NodeBB in this directory 
	$ git clone -b v2.x https://github.com/NodeBB/NodeBB.git nodebb
Head into NodeBB dir
	$ cd nodebb
Run the setup
	$ sudo ./nodebb setup
	connect MongoDB during setup with the nodebb database you created in step 3
	create an admin
	- admin
	- <some@email.com>
	- admin123

You should now be able to access your NodeBB service at http://localhost:4567
