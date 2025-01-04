# MongoDB Installation on Ubuntu (from `.tgz`)

![ScreenShot Tool -20250104005845](https://github.com/user-attachments/assets/1f1b77e8-c1d8-4964-b2a8-6d954c082ee7)

This guide will walk you through installing MongoDB on an Ubuntu system using the official `.tgz` package. It includes steps for setting up MongoDB, handling permissions, troubleshooting, and using `mongosh` for database interactions.

## Prerequisites

- Ubuntu 20.04 or later.
- A user account with sudo privileges.
- A terminal or command-line access.

## Check Compatibility

Before proceeding, make sure your system is compatible with the MongoDB version you're going to install. Check your Ubuntu version using:

```bash
lsb_release -a
```
- `MongoDB` requires a compatible version of Ubuntu. Make sure to download the .tgz package that corresponds to your system's architecture and Ubuntu version.

You can check the official `MongoDB` download page for the latest community edition releases:
`MongoDB` Downloads

### Steps
 1. Download `MongoDB`
Ensure you have downloaded the `MongoDB` .tgz archive (e.g., mongodb-linux-x86_64-ubuntu2404-8.0.4.tgz) to your Downloads directory or any other folder. You can download it directly from the `MongoDB` Download Center.

2. Install Dependencies
Before extracting and setting up `MongoDB`, you need to install the following dependencies:

```bash
sudo apt update
sudo apt install -y libcurl4 openssl
```
3. Extract the MongoDB Archive
- Next, extract the downloaded .tgz file:

```bash
cd ~/Downloads
tar -xvzf mongodb-linux-x86_64-ubuntu2404-8.0.4.tgz
```
- This will extract the MongoDB files into a directory named mongodb-linux-x86_64-ubuntu2404-8.0.4.

4. Move MongoDB Files to /opt Directory
- Move the extracted MongoDB files to the /opt directory for better organization:

```bash
sudo mv mongodb-linux-x86_64-ubuntu2404-8.0.4 /opt/mongodb
```
5. Set Up MongoDB Directories
- Create the necessary directories for MongoDB’s data storage and logs:

```bash
sudo mkdir -p /data/db
sudo mkdir -p /var/log/mongodb
```
6. Adjust Permissions
- Ensure that MongoDB has the proper permissions to read/write in the directories:

```bash
sudo chown -R $(whoami):$(whoami) /data/db
sudo chown -R $(whoami):$(whoami) /var/log/mongodb
```
7. Start MongoDB
- Now, you can start the MongoDB server. Use the following command to start MongoDB:

```bash
mongod --dbpath /data/db --logpath /var/log/mongodb/mongod.log
```
- Note: If you encounter issues with permissions, ensure that the /data/db/journal directory also has the correct permissions:

```bash
sudo mkdir -p /data/db/journal
sudo chown -R $(whoami):$(whoami) /data/db/journal
```
8. Verify MongoDB is Running
- Check if MongoDB is running:

```bash
ps aux | grep mongod
```
- You should see the mongod process running.

9. Access MongoDB with mongosh
- Now that MongoDB is running, you can interact with it using the Mongo shell:

```bash
mongosh
```
- This will connect you to the running MongoDB instance. You can now perform database operations.

Note: If you only have the old mongo shell installed, you can install it with:

```bash
sudo apt install mongodb-clients
```
10. Configure MongoDB (Optional)
- You may want to configure MongoDB to run automatically on startup or adjust other settings. For security, enabling authentication and access control is recommended for production environments.

### Enabling Authentication:
- To enable authentication, add the --auth option when starting MongoDB:

```bash
mongod --auth --dbpath /data/db --logpath /var/log/mongodb/mongod.log
```
#### Remote Access:
- If you want to allow remote access, bind MongoDB to all interfaces:

```bash
mongod --bind_ip_all --dbpath /data/db --logpath /var/log/mongodb/mongod.log
```
Alternatively, you can specify a particular IP address for remote access.

12. Stop MongoDB Server
- If you want to stop the MongoDB server, you can do so in a few different ways:

#### Using mongod Process ID (PID)
- You can stop MongoDB by finding its process ID (PID) and killing the process:

```bash
ps aux | grep mongod
```
- This will show you the mongod process and its PID. Once you have the PID, you can stop the server with:

```bash
sudo kill <PID>
```
- Replace <PID> with the actual process ID of the mongod process.

12. Using systemctl (If MongoDB is configured as a service)
- If MongoDB is set up as a system service, you can stop it using the systemctl command:

```bash
sudo systemctl stop mongod
```
3. Using Ctrl + C (If Running in Foreground)
- If MongoDB is running in the foreground (i.e., you started it manually in the terminal), you can stop it by pressing Ctrl + C in the terminal window where the mongod process is running.
