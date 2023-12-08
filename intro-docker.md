# Intro to Docker

Docker ensures code consistency across different environments. It packages all necessary dependencies, including the correct version of runtime environments like Node.js, so that applications run identically everywhere.

- **Example**: If your computer has Node version 14 and another person's machine has Node 9, Docker can include Node 14 in its package to ensure compatibility.

## Usage

- To create an isolated environment, run: `docker-compose up`
- To dispose of the environment, use: `docker-compose down --rmi all`

Docker is instrumental in building, running, and shipping applications consistently.

### Container

- A container is an isolated environment for running an application.

### VMs vs Containers

#### Problems with VMs

- Each VM requires a full-blown operating system.
- Slower to start and more resource-intensive.

#### Advantages of Containers

- Allow running multiple apps in isolation.
- Lightweight and use the host's OS (only one OS to manage/patch).
- Quick to start and require fewer hardware resources.

## OS Compatibility

- Each OS has a unique kernel, which is why a Windows application typically cannot run on Linux.
- On Linux, you can only run Linux containers.
- Windows supports both Linux (via WSL2) and Windows containers.
- macOS uses a Linux VM to run Docker containers due to the lack of native container support in its kernel.

# How to Install Docker

Below are the latest Docker installation guides for various operating systems. Ensure you're following these steps to use the most up-to-date key management methods.

## Windows

(TODO: Research and provide the latest Docker installation instructions for Windows)

## Linux

(TODO: Research and provide the latest Docker installation instructions for Linux)

## WSL2

(TODO: Research and provide the latest Docker installation instructions for WSL2)

## macOS

(TODO: Research and provide the latest Docker installation instructions for macOS)

### Dev Workflow when using docker:

- Take an application and dockerize it. We make a small change so it can be run by docker. We do this by adding a dockerfile to the directory of our project.

- This file allows docker to package up our application and put it into an image. in the image we have a cut down OS, a runtime environment (eg Node), application files, third party libraries, environment variables

- Once we have image, we tell docker to start a container using that image. so instead of directly running the application on our machine, we tell docker to run it as an isolated environment with the command docker run.

- Once we have the docker image we can push it to a docker registery like docker hub. Once our app is on the docker hub, we can put it on any machine running docker. remember, the image holds all of the project info and requirements/dependencies so this allows us to have the same dev environment no matter what computer were working on or who you share the docker image with. all the instructions for building the project live in the docker image we created.

### TYPICAL DEV WORKFLOW:

```bash
	mkdir hello-docker
	cd hello-docker
	code app.js
```

Once in vscode write one line of java in the code.

```java
console.log("Hello Docker!");
```

Now we can save the file. now, typically if we wanted to run this file we would go to our terminal where the app.js file is and run the command node.app.js. 

```shell
node app.js
Hello Docker!
```

If i wanted to send this app to you this would be what you need.

- You need an OS
- Node installed with compatible version
- Copy app files
- run node app.js

This isn't too crazy but imagine a complex project. it would be a nightmare to setup all the dependencies and requirements manually.

### Now with Docker:

Back on the app.js project folder, create a new file in vscode and call it Dockerfile ( doesn't have an xtension)

vscode will ask if you want to install Docker extension, say yes.

In the docker file were going to write the instructions for creating our shareable image.

Typically we start from a base image. These base images are officially published on docker hub. you can find them by searching with the docker desktop app you should've downloaded.

Were going to use the node base image. In the dockerfile in vscode start with this line

```
FROM node:alpine
```


This says we want to use a base node image running a version of linux called alpine. linux is opensource and has a bunch of different versions. alpine is a efficient and small image thats good for containers so we will use it.

Next we need to COPY all of our current files in the current directory into the app directory in the image.

next line to add in the code is 

```
COPY . /app
```

This command tells docker we want to copy all files into a directory in our docker image called app.

Next we will use the CMD command to tell the docker image what file we want to run. in this case we only have one file so we would run it as

```
CMD node /app/app.js
```

Remember the directory needs to have the /app/ before the file name because thats where our files live in the docker image*

If you dont want to worry about adding the /app/ before the file names, you can set a WORKDIR before the CMD command line. this way your docker file knows what directory to check by default. this is how it would look complete

```
FROM node:alpine
COPY . /app
WORKDIR  /app
CMD node app.js
```

Now in the terminal, we want to tell docker were done and ready to package up the image.

type 

```
docker build -t hello-docker .
```

Docker build will package up and create our image. the -t signifies a tag name for the created image which we named it hello-docker. then the period is the location where we can find the dockerfile. since we are already in the correct directory, we can just use a period to signify we want it to use the dockerfile in our current directory.

The docker file will build and run through code. you may expect some new docker image to appear in your directory but it wont. thats not how docker works. the process is quite complex and its something we dont have to cover right now.

Go back to your terminal. to see all docker images that exist you can type docker image ls. (ls is short for list)

It will show you all the docker images on your computer. you should see the new hello-docker file we created. it will add a TAG called latest. this is for version control. we will cover this later. each image has an IMAGE_ID which is a unique identifier. It will also show when the image was created as well as the SIZE of the image. we used alpine linux so it will be around 100mb or so. image would be larger if we used a bigger version of linux prior during setup.

Now that we identified our image exists, we can run in the terminal 

```
docker run hello-docker. 
```

You will see it prints: Hello Docker! 

Since when we created the dockerfile we said we wanted it to run the CMD node app.js so it did and printed the output!

Now that the image exists on docker hub, we can go to docker hub and search for it, and you will find it. now anyone can download the image and run it on their own computer and have the same environment and expect the same results as you got when you ran it locally.

Play with Docker! https://labs.play-with-docker.com/

Play with docker is a virtual machine that you run in your browser. it lets you test docker images! the virtual machine is blank and only contains linux and docker. so if you tried to run your application the old fashioned way it wouldnnt work because it doesnt have node installed.

Now you will see the power of docker!

In the play with docker website, click new instance. let it load and then in the terminal type 

```
docker pull <name of your docker image file> 
```

For example, for me i would type the following since my Docker account is xjbar and I named my image hello-docker

```
docker pull xjbar/hello-docker
```

Now in the same terminal run to see that your image was pulled successfully!

```
docker image ls
```

You should see your file!

Now you can run it the same way you ran it on your local machine.

```
docker run <name of your docker image file>
```

For example i would run

```
docker run xjbar/hello-docker
```

You will see the output is 

```
Hello Docker!
```

I hope this introduction to Docker has been helpful! Message me on Discord @ j.bar for any questions or collaboration!