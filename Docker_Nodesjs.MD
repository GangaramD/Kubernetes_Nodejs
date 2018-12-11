# Dockerizing a Node.js web app
The goal of this example is to show you how to get a Node.js application into a Docker container

Create the Node.js app:
 Lets Create a Directory where we will be storing all the Files.
 
                  $ mkdir Node
                  $ cd Node
    
 Create a Package.json File,
 
 $ vi Package.json
 
                      {
                  "name": "docker_web_app",
                  "version": "1.0.0",
                  "description": "Node.js on Docker",
                  "author": "First Last <first.last@example.com>",
                  "main": "server.js",
                  "scripts": {
                    "start": "node server.js"
                  },
                  "dependencies": {
                    "express": "^4.16.1"
                  }
                }

Now Run npm Install

                $ npm install

It will download all the dependency files and modules for Node.js application

create a server.js file that defines a web app using the Express.js framework

 $ vi server.js
 
                'use strict';

                const express = require('express');

                // Constants
                const PORT = 8080;
                const HOST = '0.0.0.0';

                // App
                const app = express();
                app.get('/', (req, res) => {
                  res.send('Hello world\n');
                });

                app.listen(PORT, HOST);
                console.log(`Running on http://${HOST}:${PORT}`);
                
                
  Now we will configur how run this app inside a Docker container using the official Docker image. 
  First, you'll need to build a Docker image of your app.
  
  Create a Dockerfile
  
  $ vi Dockerfile
  
                  FROM node:8
                  # Create app directory
                  WORKDIR /usr/src/app

                  # Install app dependencies
                  # A wildcard is used to ensure both package.json AND package-lock.json are copied
                  # where available (npm@5+)
                  COPY package*.json ./

                  RUN npm install
                  # If you are building your code for production
                  # RUN npm install --only=production

                  # Bundle app source
                  COPY . .

                  EXPOSE 8080
                  CMD [ "npm", "start" ]
                  
Create a .dockerignore file in the same directory as your Dockerfile with following content.
This will prevent your local modules and debug logs from being copied onto your Docker image and possibly overwriting modules installed within your image.
                  
  $ vi .dockerignore
  
                  node_modules
                  npm-debug.log
   
 Now Lets build the docker image from the Docker File
   
                  $ docker build -t <your username>/node-web-app .
    
 Your image will now be listed by Docker,
    
                  $ docker images
    
 Run the image:
 
    $ docker run -p 49160:8080 -d <your username>/node-web-app
    
 Now Test the app:
 
      $ docker ps
      
The Application is running at the port 49160, Open the URL in any of your local browser and then,

                http:localhost:49160
    
