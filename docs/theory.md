# Inception: The Theory

---

## Resources

https://medium.com/@imyzf/inception-3979046d90a0

---

## What is Docker
Tool designed to allow to build, deploy and run applications in an isolated and consistent manner across different machines and OSs.

- This process is done using **Containers**
	- lightweight virtualized enviroments
	- package all dependencies and code an app needs to run into a single text file
		- run the same way on any machine
- Can also be used to create and run other types of containers:
	- testing, development and experimentation.

Although it's not guarantee error-free performance, it can reduce the likelyhood of unexpected errors.

---

## What is Docker Image

Lightweight executable package that includes everything the app needs to run:
- Code
- Runtime environment
- System tools
- Libraries
- Dependencies.

It is build from **Dockerfile**, which is a simple text file that contains a set of instructions for building the image, with each instruction creating a new layer in the image.

---

## What is Dockerfile

- Simple text file.
- Contains a set of instructions to build Docker image.

Specifies the base image to use (Debian / Alpine) and then includes a series of commands that automate the process for configuring and building an image:

- Installing packages
- Copying files
- Setting env variables.

Each command in the Dockerfile creates a new layer in the image.

Example:

```docker
# This Specifies the base images for the container
# (in this case, it's the 3.14 version of Alpine)
FROM alpine:3.14

# This Run commands in the container shell,
# and installs the specified packages
# (it will install nginx & openssl,
# and will create the directory "/run/nginx" as well)
RUN apk update && \
    apk add nginx openssl && \
    mkdir -p /run/nginx

# This Copies the contents of "./conf/nginx.conf" on the host
# machine to the "/etc/nginx/http.d/"
# directory inside the container
COPY ./conf/nginx.conf /etc/nginx/http.d/default.conf

# This Specifies the command that will run when the container get started
CMD ["nginx", "-g", "daemon off;"]
```

---

## What is Docker Compose

Tool that simplifies the deployment and management of multi-container Docker applications.

This tool simplifying the process of:
- Defining related services
- Volumes for data persistence
- Networks for connecting containers.

With Docker Compose, I can easily configure each service's settings, including:
- Image to use
- Ports to expose
- Env variables to set.

Docker Compose streamlines the development process, making it easier to build and deliver the applications with greater efficiency and ease.

### Example

```docker-compose
version: '3'

# All the services that you will work with should be declared under
# the SERVICES section!
services:

  # Name of the first service (for example: nginx)
  nginx:
  
    # The hostname of the service (will be the same as the service name!)
    hostname: nginx
    
    # Where the service exist (path) so you can build it
    build:
      context: ./requirements/nginx
      dockerfile: Dockerfile
      
    # Restart to always keep the service restarting in case of
    # any unexpected errors causing it to go down
    restart: always
    
    # This line explains itself!!!
    depends_on:
      - wordpress
      
    # The ports that will be exposed and you will work with
    ports:
      - 443:443
      
    # The volumes that you will be mounted when the container gets built
    volumes:
      - wordpress:/var/www/html
      
    # The networks that the container will connect and communicate
    # with the other containers
    networks:
      - web
```

### 3 parts of Docker Compose

#### Services

Service is a unit of work in Docker Compose. It has a name, and it defines a:
- Container images
- Set of env variables
- Set of ports that are exposed to the host machine

When I run `docker-compose up`, Docker will create a new container for each service in the Compose file.

#### Networks

A way for containers to communicate with each other.

When I create a network in my Compose file, Docker will create a new network that all other containers in my Compose file will be connected to.

This allows containers to communicate with each other without even knowing the IP of each other, just by name.

#### Volumes

A way to store data that is shared between containers.

When creating a volume in the Compose file, Docker will create a **new** volume that all the containers have access to.

This allows to share data between the containers without having to copy-paste each and every time you want that data.

---

## What is Container

Standard unit of software that packages up code and all its dependencies so app runs quickly and reliably from one computing environment to another.

## Container vs. VM

- Similar resource isolation and allocation benefits.
- Function differently: containers virtualize OS instead of hardware.
- Containers are more portable and efficient.

### Container

- Abstraction at the app layer that packages code and dependencies together.
- Multiple containers can run on the same machine and share the OS kernel with other containers, each running as isolated processes in user space.
- Take up less space then VMs
- Can handle more applications
- Require fewer VMs and OSs

### Virtual Machines

- Abstraction of physical hardware turning one server into many servers
- Hypervisor allows multiple VMs to run on a single machine
- Each machine includes a full copy of an OS, app, binaries, libraries.
- Take more space then containers
- Slow to boot



