
APT repository
Advanced Package Tool repository
is a collection of software packages and package information designed for use with Debian-based operating systems, such as Debian itself, Ubuntu
allows package management tools like APT to easily retrieve, install, upgrade, and remove software on a system.



create own image:

--manually
* OSe.g ubuntu
* update apt repo
* install dependencies using apt commands
* install language-based dependencies (pip, jvm)
* copy source code to /opt folder
* run the web server


--with docker image
* create a Dockerfile (write instructions to setup the application, entry point of app)
* build the docker image (specify Dockerfile s input, tag name for image)


A Dockerfile is built from instructions and arguments

COPY copies files from local machine onto the docker image
ENTRYPOINT specify a command that specifies what to run as the image runs as a container


command
-----------

available images
docker images

build image using the Dockerfile in the current working directory and name it webapp-color. Build it into the current pwd (.)
the -t flag is used to tag the Docker image that is being built.
docker build -t webapp-color .

build with a specific tag
$ docker build -t webapp-color:lite .

smaller image go for slim (search in dckerhub)

Run an instance of the image webapp-color and publish port 8080 on the container to 8282 on the host (my local machine)
docker run -p 8282:8080 webapp-color

print OS name of an image
docker run python:3.6 cat /etc/*release*

docker run -p 8383:8080 webapp-color:lite

interactive, see what's inside the instance
$ docker  run -it python:3.6 /bin/bash
exit



containers only live as long as the process inside is alive

CMD ["nginx"] -> defines which program runs inside te container when it starts

create an image on top of the existing to override
CMD ["sleep", "5"]

### commands

list all running containers
`doecker ps`

see all containers (incl. stopped)
`docker ps -a`