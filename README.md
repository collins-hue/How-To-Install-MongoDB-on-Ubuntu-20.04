# How-To-Install-MongoDB-on-Ubuntu-20.04

MongoDB is a document database used in many modern web applications. It is classified as a NoSQL database because it does not rely on a traditional table-based relational database structure.
Instead, it uses JSON-like documents with dynamic schemas, meaning that, unlike relational databases, MongoDB does not require a predefined schema before you add data to a database. You can alter the schema at any time and as often as is necessary without having to set up a new database with an updated schema.

#Prerequisites

1. One Ubuntu 20.04 server.

#Step 1 — Installing MongoDB

Ubuntu’s official package repositories include a stable version of MongoDB. However, as of this writing, the version of MongoDB available from the default Ubuntu repositories is 3.6, while the latest stable release is 4.4.
To start, import the public GPG key for the latest stable version of MongoDB by running the following command. If you intend to use a version of MongoDB other than 4.4, be sure to change 4.4 in the URL portion of this command to align with the version you want to install:

   ```
   curl -fsSL https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
   ```

This command will return OK if the key was added successfully.

If you’d like to double check that the key was added correctly, you can do so with the following command:
   ```
   apt-key list  
   ```

This will return the MongoDB key somewhere in the output:
   ```
   Output
   /etc/apt/trusted.gpg
   --------------------
   pub   rsa4096 2019-05-28 [SC] [expires: 2024-05-26]
         2069 1EEC 3521 6C63 CAF6  6CE1 6564 08E3 90CF B1F5
   uid           [ unknown] MongoDB 4.4 Release Signing Key <packaging@mongodb.com>
   . . .
   ```

At this point, your APT installation still doesn’t know where to find the mongodb-org package you need to install the latest version of MongoDB.

Run the following command, which creates a file in the sources.list.d directory named mongodb-org-4.4.list. The only content in this file is a single line reading deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse:
   ```
   echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
   ```

After running this command, update your server’s local package index so APT knows where to find the mongodb-org package:
   ```
sudo apt update
   ```
Following that, you can install MongoDB:
   ```
   sudo apt install mongodb-org
   ```

When prompted, press Y and then ENTER to confirm that you want to install the package.

#Step 2 — Starting the MongoDB Service and Testing the Database

Run the following systemctl command to start the MongoDB service:
   ```
   sudo systemctl start mongod.service
   ```
Then check the service’s status. 

sudo systemctl status mongod

This command will return output like the following, indicating that the service is up and running:
   ```
   Output
   ● mongod.service - MongoDB Database Server
        Loaded: loaded (/lib/systemd/system/mongod.service; disabled; vendor preset: enabled)
        Active: active (running) since Tue 2020-06-09 12:57:06 UTC; 2s ago
          Docs: https://docs.mongodb.org/manual
      Main PID: 37128 (mongod)
        Memory: 64.8M
        CGroup: /system.slice/mongod.service
                └─37128 /usr/bin/mongod --config /etc/mongod.conf
   ```

After confirming that the service is running as expected, enable the MongoDB service to start up at boot:
   ```
   sudo systemctl enable mongod
   ```

You can further verify that the database is operational by connecting to the database server and executing a diagnostic command. The following command will connect to the database and output its current version, server address, and port.
   ```
   mongo --eval 'db.runCommand({ connectionStatus: 1 })'
   ```

connectionStatus will check and return the status of the database connection. A value of 1 for the ok field in the response indicates that the server is working as expected:
   ```
   Output
   MongoDB shell version v4.4.0
   connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
   Implicit session: session { "id" : UUID("1dc7d67a-0af5-4394-b9c4-8a6db3ff7e64") }
   MongoDB server version: 4.4.0
   {
   	   "authInfo" : {
   		   "authenticatedUsers" : [ ],
   		   "authenticatedUserRoles" : [ ]
   	   },
   	   "ok" : 1
   }
   ```

Also, note that the database is running on port 27017 on 127.0.0.1, the local loopback address representing localhost. This is MongoDB’s default port number.

#Step 3 — Managing the MongoDB Service

As mentioned previously, the systemctl status command checks the status of the MongoDB service:
   ```
   sudo systemctl status mongod
   ```
To start the service when it’s stopped, run:
   ```
   sudo systemctl start mongod
   ```

You can also restart the server when it’s already running:
   ```
   sudo systemctl restart mongod
   ```

In Step 2, you enabled MongoDB to start automatically with the server. If you ever wish to disable this automatic startup, type:
   ```
   sudo systemctl disable mongod
   ```

Then to re-enable it to start up at boot, run the enable command again:
   ```
   sudo systemctl enable mongod
   ```

#Conclusion

In this tutorial, you added the official MongoDB repository to your APT instance, and installed the latest version of MongoDB. You then tested Mongo’s functionality and practiced some systemctl commands.

As an immediate next step, we strongly recommend that you harden your MongoDB installation’s security by following our guide on How To Secure MongoDB on Ubuntu 20.04. Once it’s secured, you could then configure MongoDB to accept remote connections.

#Reference

1. https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-20-04

2. https://www.digitalocean.com/community/tutorials/how-to-secure-mongodb-on-ubuntu-20-04

3. https://www.digitalocean.com/community/tutorials/how-to-configure-remote-access-for-mongodb-on-ubuntu-20-04

4. https://www.digitalocean.com/community/search?q=mongodb
