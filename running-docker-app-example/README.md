# Node.js with MongoDB and Docker Demo

Application demo designed to show how Node.js and MongoDB can be run in Docker containers.
The app uses Mongoose to create a simple database that stores Docker commands and examples.

### Starting the Application with Docker Containers:

1. Install Docker for Windows or Docker for Mac (If you're on Windows 7 install Docker
   Toolbox: http://docker.com/toolbox).

2. Open a command prompt.

3. Run the commands listed in `node.dockerfile` (see the comments at the top of the file).

4. Navigate to http://localhost:3000. Use http://192.168.99.100:8080 in your browser to view the site if using Docker
   toolbox. This assumes that's the IP assigned to VirtualBox - change if needed.

### Starting the Application with Docker Compose

1. Install Docker for Windows or Docker for Mac (If you're on Windows 7 install Docker
   Toolbox: http://docker.com/toolbox).

2. Open a command prompt at the root of the application's folder.

3. Run `docker-compose build`

4. Run `docker-compose up`

5. Open another command prompt and run `docker ps -a` and note the ID of the Node container

6. Run `docker exec -it <nodeContainerID> sh` (replace <nodeContainerID> with the proper ID) to sh into the container

7. Run `node dbSeeder.js` to seed the MongoDB database

8. Type `exit` to leave the sh session

9. Navigate to http://localhost:3000 (http://192.168.99.100:3000 if using Docker Toolbox) in your browser to view the
   site. This assumes that's the IP assigned to VirtualBox - change if needed.

10. Run `docker-compose down` to stop the containers and remove them.

## To run the app with Node.js and MongoDB (without Docker):

1. Install and start MongoDB (https://docs.mongodb.com/manual/administration/install-community/).

2. Install the LTS version of Node.js (http://nodejs.org).

3. Open `config/config.development.json` and adjust the host name to your MongoDB server name (`localhost` normally
   works if you're running locally).

4. Run `npm install`.

5. Run `node dbSeeder.js` to get the sample data loaded into MongoDB. Exit the command prompt.

6. Run `npm start` to start the server.

7. Navigate to http://localhost:3000 in your browser.

### Other useful Commands

1. Building a Custom Image: <br>`docker build -t <name> .`

```txt
   -t --> short for --tag
   <name> --> Image name
   . --> Build context 
```

2. Using Registry Name, Image Name, and Tag
   `docker build -t <registry>/<name>:<tag> .`

```txt
   -t --> short for --tag
   <registry> --> Docker registry name
   <name> --> Image name
   . --> Build context 
```

3. Docker Image Commands: <br>
   `docker images` -> List Docker images <br>
   `docker rmi <imageId>` -> Remove an Image
4. Deploy an Image to Docker Hub:<br>
   `docker push <user name>/<image name>:<tag>`

5. Pulling an Image from Registry<br>
   `docker pull <imageName>`
6. Running Container<br>
   `docker run -p <extenalPort>:<internalPort> <imageName>`

### Using docker run

1.`docker pull nginx:alpine`

2. `docker run -p 8080:80 -d nginx:alpine`
3. `docker ps -a`

### User docker logs to View Container Logs

`docker logs <containerId>`

### Creating a Container Volume

1. `docker run -p <ports> -v /var/www/logs <imageToRun>` <br>

```txt
-v /var/www/logs --> Data in folder should be stored on the container host
```

2. Defining a Host Location (Mac/Linux): <br>
   `docker run -p <ports> -v $(pwd):/var/www/logs <imageToRun>`
3. Defining a Host Location (Windows): <br>
   `docker run -p <ports> -v ${PWD}:/var/www/logs <imageToRun>`
   <br>

#### Creating a Container Volume (example):

1. Create nginx folder
2. Create index.html with some html content
3. `docker run -p 8080:80 -v $(pwd):/usr/share/nginx/html nginx:alpine`
4. run `localhost:8080` -> it will change to our created index.html :).

