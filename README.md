---
title: 'Docker Container with NodeJS'
author: Mohammed Jamal
---

### 1. Let suppose we have a nodejs folder wich is already set with html and nodejs project
inside nodejs folder root app, create a Dockerfile and inside it put below code **``path to project: ~/node_project/Dockerfile``**

```
FROM node:10-alpine

RUN mkdir -p /home/node/app/node_modules && 
chown -R node:node /home/node/app

WORKDIR /home/node/app

COPY package*.json ./

USER node

RUN npm install

COPY --chown=node:node . .

EXPOSE 8080

CMD [ "node", "app.js" ]
```
<br>



### 2. Now, create .dockerignore file inside project root folder
**``~/node_project/.dockerignore``**
```
node_modules
npm-debug.log
Dockerfile
.dockerignore
```

### 3. Open Terminal and build application image
**``$ docker build -t your_dockerhub_username/nodejs-demo .``**

### 4. Checks your images
**`` $ docker images ``**


### 5. Create a container with your images
**`` $ docker run --name nodejs-image-demo -p 80:8080 -d your_dockerhub_username/nodejs-demo  ``**


### 6. inspect a list of your running containers with
**`` $ docker ps ``**

With your container running, you can now visit your application 
by navigating your browser to ``` http://your_server_ip.``` 
You will see your application landing page.



### 7. Using a dockerhub Repository to Work with Images
By pushing your application image to a registry like Docker Hub, you make it available for subsequent use as you build and scale your containers. 

**``$ docker login -u your-dockerhub-username``**

When prompted, enter your Docker Hub account password. Logging in this way will create a ~/.docker/config.json file in your user's home directory with your Docker Hub credentials.
You can now push the application image to Docker Hub using the tag you created earlier, your_dockerhub_username/nodejs-image-demo:

**``$ docker push your-dockerhub-uername/nodejs-demo``**


* Let's test the utility of the image registry by destroying our current application container and image and rebuilding them with the image in our repository.
    - First, list your running containers:
        
        **``$ docker ps``**

    - Using the CONTAINER ID listed in your output, stop the running application container. 

        **``$ docker stop e50ad27074a7``**
    - List your all of your images with the -a flag:
    
        **``$ docker images -a``**

    - You will see the following output with the name of your image, your_dockerhub_username/nodejs-image-demo, along with the node image and the other images from your build.
    - Remove the stopped container and all of the images, including unused or dangling images, with the following command:
        
        **``$ docker system prune -a``**

    - Type y when prompted in the output to confirm that you would like to remove the stopped container and images. Be advised that this will also remove your build cache.
    - you have now removed both the container running your application image and the image itself.
    
    - With all of your images and containers deleted, you can now pull the application image from Docker Hub: 
        **``$ docker pull your_dockerhub_username/nodejs-image-demo``**

    - List your images once again:
        
        **``$ docker images``**
    
    - You can now rebuild your container using the command from Step 3:
        
        **``$ docker run --name nodejs-image-demo -p 80:8080 -d your_dockerhub_username/nodejs-image-demo``**
    
    - List your running containers:

        **``$ docker ps``**
    - Visit **http://your_server_ip** once again to view your running application.
