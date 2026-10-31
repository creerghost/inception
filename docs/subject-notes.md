# Inception

## Object

- Set up a small infrastructure composed of different services under certain rules
- Whole project should be done in VM
- Have to use `docker compose`

### Docker images

- Each image should have the *same name* as corresponding service
- Each service runs in separate container
- Containers must be built either from the *penultimate stable* version of Alpine OR Debian
- I have to build Docker images myself
- It is forbidden to pull ready-made Docker images
- It is forbidden to use DockerHub (Alpine/Debian is excluded from this rule)

### Dockerfile

- I have to write my own Dockerfile for each service
- They must be called in my `docker-compose.yml` by my `Makefile`
	- network line must be present in compose file
	- host or --link or links are forbidden
- I have to configure my domain name so it points to my local IP adress
- Domain name must be `<login>.42.fr`
	- Latest tag is prohibited
	- No passwords in Dockerfiles
	- Mandatory to use .env and env variables
	- Recommended to use Docker secrets to store any confidential information

## Setup of Docker Containers

- NGINX with TLSv1.2 / TLSv1.3
	- Must be the only entrypoints into my infrastructure via port `443` only, using TLS protocols
- WordPress + php-fpm (it must be installed and configured)
	- without nginx
- MariaDB
	- without nginx
- A volume that contains my WordPress database
	- Must use Docker **named volumes**
	- Must store their data inside /home/<login>/data on the host machine
- Second volume that contains my WordPress website files
	- Must use Docker **named volumes**
	- Must store their data inside /home/<login>/data on the host machine
- `docker-network` that establishes the connection between my containers
> They have to restart in case of a crash!
> Containers must not be started with a command running an infinite loop
> forbidden: tail -f, bash, sleep infinity, while true
> no tail -f  or simular when running containers

## Things to read

- PID 1
- best practices for writing Dockerfiles
- Daemons and whether it's a good idea to use them

