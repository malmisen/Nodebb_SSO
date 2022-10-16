## Step 1: Install Node.js ##

1. (Installing from the NodeSource rep)
	1. $ apt-get update
	2. $ apt-get upgrade
2. install the Node.js
	1. $ apt-get install curl git gcc g++ make
	2. $ curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
	3. $ apt-get install nodejs
	
## Step 2: Install MongoDB ##

MongoDB is the default database for NodeBB. 
1. Start the installation by importing the public key used by the package management system.
	1. $ apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
2. Add the MongoDB repository:
	1. $ echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb.list
3. Update the apt package index and install the MongoDB server:
	1. $ apt-get update
	2. $ apt-get install -y mongodb-org
4. Start the MongoDB service:
	1. $ systemctl start mongod.service
	2. $ systemctl enable mongod.service

## Step 3: Configure MongoDB ##

1. Log in to MongoDB running the following commands:
	1. $ mongo
2. Then switch the db to ‘admin’ and create a new admin user…
	1. use admin
3. Create a new admin user named ‘admin’ with a new password…
	1. db.createUser({ user: "admin", pwd: "admin123", roles: [ { role: "readWriteAnyDatabase", db: "admin" }, { role: "userAdminAnyDatabase", db: "admin" }]})
4. Next, create a new database called nodebb
	1. use nodebb
5. Then create a new NodeBB user named ‘nodebbuser’ with rights to administer the database…
	1. db.createUser({user:"nodebbuser",pwd:"nodebb123",roles:[{role:"readWrite",db:"nodebb"},{role:"clusterMonitor",db:"admin"}]})
Note (Might be smart to replace pwd: "admin123" and "nodebb123"  to something else)
6. After that, exit the MongoDB shell.
	1. quit()
7. After that, run the commands below to open MongoDB config file…
	1. $ sudo vim /etc/mongod.conf
	#security
	security:
	authorization:enabled

## Step 4: Install Nginx ##
	1. $ sudo apt-get install nginx
	2. $ sudo systemctl start nginx.service
	3. $ sudo systemctl enable nginx.service

## Step 5: Installing NodeBB ##
1. Go to the newly created directory by executing:
	1. $ cd /var/www
2. Clone NodeBB in this directory 
	1. $ git clone -b v2.x https://github.com/NodeBB/NodeBB.git nodebb
3. Head into NodeBB dir
	1. $ cd nodebb
4. Run the setup
	1. $ sudo ./nodebb setup
	2. connect MongoDB during setup with the nodebb database you created in step 3
	3. create an admin
		- admin
		- <some@email.com>
		- admin123

You should now be able to access your NodeBB service at http://localhost:4567
