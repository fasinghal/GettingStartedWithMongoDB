# GettingStartedWithMongoDB
This repository contains notes related to MongoDB
<hr>

### Installation Steps in Windows

1. Go to https://www.mongodb.com
2. Go to downloads
3. Select Community server OR navigate to https://www.mongodb.com/download-center?jmp=nav#community
4. Select the first version appears on the dropdown menu i.e. **Windows server 2008 R2 64-bit and later, with SSL support x64**
5. Once you have downloaded the installer, open it and click next, select **complete setup** option to install.

<br>

### Starting the MongoDB Server

- After the software is installed, navigate into command prompt and boot up the server. 
  - Navigate into program files -> MongoDB -> server -> 3.6 -> bin
    This folder contains all executables to start up the server and connect to it
    Example-> mongod.exe => This starts local MongoDB database
    
<img src = "https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/Exploring%20bin%20folder.png">

- Before we execute any executable, we need to create a directory where all of our data can get stored. For this, I have created a folder  named **mongo-data** in my user directory where the data is going to get stored. 
  - This is the path we need to specify when we execute mongod.exe (We need to tell the mongo where to store the data)

<img src = "https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/Command%20to%20start%20%20server.png" >

- By executing this command, you start up the server
<br>
<img src ="https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/Server%20has%20started.png">

