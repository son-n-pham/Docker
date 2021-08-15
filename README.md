- [Install Docker for Desktop](#install-docker-for-desktop)
- [USING DOCKER](#using-docker)
  * [The Docker Flow](#the-docker-flow)
    + [Here is an example.](#here-is-an-example)
  * [Docker run options:](#docker-run-options-)
  * [Manage Containers](#manage-containers)
  * [Exposing ports:](#exposing-ports-)
  * [Container Network](#container-network)
  * [Images](#images)
  * [Volumes](#volumes)
    + [Sharing data with the Host: It is persistent. ***-v*** option is used to achieve this goal.](#sharing-data-with-the-host--it-is-persistent-----v----option-is-used-to-achieve-this-goal)
    + [Sharing data between containers: It is ephemral. As long as there is one container with the shared folder, the shared folder still exist.](#sharing-data-between-containers--it-is-ephemral-as-long-as-there-is-one-container-with-the-shared-folder--the-shared-folder-still-exist)
  * [Docker registries](#docker-registries)
- [Building Docker Images](#building-docker-images)
  * [What are Dockerfiles?](#what-are-dockerfiles-)
  * [Building Dockerfiles](#building-dockerfiles)
  * [Dockerfile syntax](#dockerfile-syntax)
  * [Multiproject Dockerfiles:](#multiproject-dockerfiles-)
- [Orchestration: Building Systems with Docker](#orchestration--building-systems-with-docker)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>



# Install Docker for Desktop
https://hub.docker.com/

Ater intallation, run the below to check:

```docker
docker info
```

```docker
docker run hello-world
```

![image](https://user-images.githubusercontent.com/79841341/128884631-51165571-9d4f-4008-978b-3134cd2f1a18.png)

# USING DOCKER

## The Docker Flow

To view existing images, run the below

```docker
docker images
```

![image](https://user-images.githubusercontent.com/79841341/128885870-5a32e4ce-ee78-4569-b80d-7090ce7f529d.png)

Docker run takes the image and turn it into running container.
![image](https://user-images.githubusercontent.com/79841341/128886251-602d8467-b740-4612-953c-348663cc3269.png)

![image](https://user-images.githubusercontent.com/79841341/128887913-4423dcef-7902-4d58-a835-3d3bdf980cec.png)

```docker
docker run -ti cybernemosynce/cits1003:bash
```

![image](https://user-images.githubusercontent.com/79841341/128888775-63d13bce-29c5-4006-9d56-3e3bbdccb443.png)

docker ps is for checking what images are running.

![image](https://user-images.githubusercontent.com/79841341/128888904-05e1686e-bce4-4377-8257-fd27b8544abf.png)

IMPORTANT: ID from ps is the Container ID different from Image ID

We can exit a running container by **exit** command. After exiting, that container is stopped. we can run the below to check all of the images, including the images from stopped container.

```docker
docker ps -a
```

![image](https://user-images.githubusercontent.com/79841341/128890734-0a732165-4a89-494d-b98d-697188c9689b.png)

or the below gives the most recent container

```docker
docker ps -l
```

We can commit the stopped container by its Container ID to make a new image, which is containing all changes in that container.

![image](https://user-images.githubusercontent.com/79841341/128890908-0895f35f-0917-403e-9221-0e76cdfadc7e.png)

![image](https://user-images.githubusercontent.com/79841341/129070133-c23680ed-9476-41b7-ade5-b7c7c2f8bc4d.png)

### Here is an example.

I run an image, create file MY-VERY-IMPORTANT-FILE, exit, run *docker ps -l* (to have the info of the recent container).

![image](https://user-images.githubusercontent.com/79841341/128891757-0815ca78-0e35-4a72-8bfa-2af43244e4a1.png)

Then I run docker commit to create the new image with all saved info/data from the recent container by using he Container ID. Then docker tag is used to give the name to the new image.

![image](https://user-images.githubusercontent.com/79841341/128892581-413c9512-03d4-4e9a-bac3-e171e090a9c9.png)

The shortcut is just using docker commit with Container Names and give the new image name.

![image](https://user-images.githubusercontent.com/79841341/128893248-f7ac6107-9fcc-49ff-8962-e643661ab8ff.png)

## Docker run options:

--rm will remove container after exit the container
```docker
docker run --rm -ti my-image:latest
```

-d will automatically detach the running container. We can attach it back later.

```docker
docker run --rm -ti my-image:latest

# Container will be automatically detached and run in the background
# docker ps is run to retrieve that container info
docker ps

# docker attach is run with the container name to re-attach it.
docker attach hard_core_elbakyan

# when running container, we can use Ctrl+p Ctrl+q to detach it.
```

![image](https://user-images.githubusercontent.com/79841341/129030313-e1c61289-63db-4fc2-8869-2e419fec1e8a.png)

![image](https://user-images.githubusercontent.com/79841341/129030447-a8bfd648-a89d-421a-9e9d-45f92ef6f9f3.png)

***docker exec*** can be used to start another process in an existing container. But we cannot add ports, volume etc.

![image](https://user-images.githubusercontent.com/79841341/129031197-ac4a63fa-1994-4679-98e6-da376f9b465d.png)

-c to run the command after opening the container

![image](https://user-images.githubusercontent.com/79841341/129032209-904885c4-61da-4c93-8549-b2e0414db55e.png)

## Manage Containers

***docker logs*** show all the whole log of a specific container.

```docker
docker logs CONTAINER
```

![image](https://user-images.githubusercontent.com/79841341/129031724-e16e1fa2-0dc5-4787-aa64-91ac25b6a432.png)

--name gives the name for the container

***docker kill*** stops a container, ***docker rm*** remove a container

![image](https://user-images.githubusercontent.com/79841341/129033546-1ef69a39-3218-431f-9804-e1f89579b269.png)

## Exposing ports:

Docker allow several options for network.
- For internal:
  - Programs in containers are isolated from the internet by default
  - We can group our containers into "private" network
  - We explicitly select who can connect whom
- For outer:
  - Docker allows to expose ports to let connections in
  - private network to connect between containers.

Exposing a specific port:
- Explicitly specifies the port inside the container and outside
- Exposes as many ports as we want
- Requires coordination between containers
- Makes it easy to find the exposed ports


```docker
# Example: The below will open image ubuntu, and also expose 2 ports with the same inner and outer port numbers,
# name was given to the container and the container is running in the terminal
# interactive mode (-ti). The container will be removed after it is stopped.
docker run --rm -ti -p 45678:45678 -p 45679:45679 --name echo-server ubuntu

# Inside docker container, run "nc -lp 45678 | nc -lp 45679"
# for this container to listening to its exposed ports
nc -lp 45678 | nc -lp 45679
```

![image](https://user-images.githubusercontent.com/79841341/129150543-e3b39fff-8e41-4ce4-8855-e7d79ec2be8e.png)


2 bash terminals connect to docker port through localhost

![image](https://user-images.githubusercontent.com/79841341/129150600-e648e57a-c271-453b-a3b4-10758e4f349c.png)

For other containers connecting to the exposed ports, those containers need to use special "host.docker.internal", then the outer port numbers.

![image](https://user-images.githubusercontent.com/79841341/129212053-2535548c-5504-4e3c-adc0-b8307cecccbe.png)

We can expose ports dynamically by fixing the port inside only.

```docker
docker run --rm -ti -p 45678 -p 45679 --name echo-server ubuntu_nc
```
In this case, in other terminals, we need to run "docker port" with the exposed container to know the port outside to connect. Syntax will be similar to the below.

```docker
docker port echo-server
```

![image](https://user-images.githubusercontent.com/79841341/129213309-427f1910-ad05-473b-91cc-09f8744287ed.png)

The above are to expose TCP port. Docker can expose UDP ports also. TCP is the default option, for the UDP, we can use the universal syntax below:

```docker
docker run -p outside-port:inside-port/prototol (tcp/udp)
```

The nc tool just needs to have addition -u option to handle udp.

![image](https://user-images.githubusercontent.com/79841341/129214617-a77ad095-71a3-4f29-93e1-48f9a77c1003.png)

## Container Network

To list all available network:

```docker
docker network ls

```

![image](https://user-images.githubusercontent.com/79841341/129215226-5284b2cb-466f-4ee9-8c1e-0170f4ef940c.png)

bridge, host and none are default network. Bridge is to connect other server without preferences, host is too open and have no network isolation while none is no network at all.

***docker network create NETWORK_NAME*** can be used to create a new network

```docker
docker network create learning
```

Then we can run the container on the new network ***learning*** by adding ***--net learning***. As the result, we can ping containter catserver in network learning. However, the attempt to ping dogserver is not working as it is not existing.

```docker
docker run --rm -ti --net learning --name catserver ubuntu20
```

![image](https://user-images.githubusercontent.com/79841341/129219663-a12ec05b-3cda-4798-b03e-d1f51444a301.png)

Now, on the right, we create the new dogserver container. Now the left of catserver can ping dogserver on the right and vice versa. We can run nc with the dogserver name instead of the default "host.docker.internal". This is one network with 2 machines (servers)

![image](https://user-images.githubusercontent.com/79841341/129221274-0bc2bbc9-c502-49f7-a3b9-cdb1a2d75e72.png)

Now it is more interesting when we make another network catsonly

```docker
docker network create catsonly
```

![image](https://user-images.githubusercontent.com/79841341/129222044-c8d5b5fe-8737-4986-af8d-2a889e3dcc8e.png)

We can run the below to connect catserver in learning network to the new network catsonly.

```docker
docker network catsonly catserver
```

When connecting catserver to catsonly network, catserver can connect both container in catsonly and learning networks. Other containers in those 2 network cannot communicate with others in the other side.

## Images

***docker images*** to list all images

***docker commit CONTAINER_NAME/ID NEW_IMAGE_NAME:TAG***

Image can come from ***docker pull*** or are automatically downloaded when we use ***docker run*** but that image is not in the device yet.

Clearning up image by ***docker rmi***

***docker tag*** can be used to give names to images

## Volumes

It is used to sharing data between containers or containers and host. Volumes are not part of images, and can be:
- Persistent: Still exist after containers are closed.
- Ephemeral: Disappear when containers are gone.

### Sharing data with the Host: It is persistent. ***-v*** option is used to achieve this goal.

```docker
docker run --rm -ti -v HOST_FOLDER:CONTAINER_FOLDER --name echo-server ubuntu20
```

![image](https://user-images.githubusercontent.com/79841341/129377882-29da44c8-87f0-4ba0-ad31-afbc07664092.png)

### Sharing data between containers: It is ephemral. As long as there is one container with the shared folder, the shared folder still exist.

![image](https://user-images.githubusercontent.com/79841341/129382841-44afec7c-88d1-4d40-af8e-cc633acb4483.png)

## Docker registries

***docker search*** command to search images

```docker
docker search ubuntu
```

We can also search images in hub.docker.com

***docker login*** command to log in docker in CLI

***docker push*** command can publish docker images

# Building Docker Images

## What are Dockerfiles?
- a small "program" to create an image
- Run with the below:
  - Use docker build to run the Dockerfiles
  - -t for tag to give the tag after the run
  - When finished, result is in the local docker registry
  - Each line in Dockerfile takes the previous image from the previous line and make another image. The previous image is unchanged => don't want dockerfile with large file and lots of lines
  - Dockerfiles are not shell scripts. Processes on oneline will not be running on the next line. If we want to execute both of them, we need to put them on the same line.
```Docker
docker build -t name_of_result folder_where_to_find_dockerfile
```

## Building Dockerfiles
Build a Dockerfile in the current working folder, its content is as below:
- First line: Build the new image from image debian:sid
- Second line: run apt update in the new container. -y is to say yes when the update command asks y/n
- Third line: install nano program
- Forth line: run nano program to create file notes in /tmp folder.
```Docker
FROM debian:sid
RUN apt-get -y update
RUN apt-get -y install nano
CMD ["/bin/nano", "/tmp/notes"]
```

Run the new Dockerfile
```Docker
docker build -t example/nanoer .
```
![image](https://user-images.githubusercontent.com/79841341/129466950-c537f753-ad06-44b2-8a0b-0325354655e5.png)

We can add file from our current folder into the new image from the Dockerfile by creating a new DOckerfile

![image](https://user-images.githubusercontent.com/79841341/129467099-6e2fddf9-a44a-44e2-ba36-5e1c853a7cdf.png)

## Dockerfile syntax
- FROM statement
- MAINTAINER statement
  - Define the author of this Dockerfile
  - Syntax: MAINTAINER Firstname Lastname <exmail@example.com>
- RUN statement: Runs the command line, wait for it to finish and saves the result.
  - Example: RUN unzip install.zip /opt/install/
- ADD statement:
  - Adds local files: Add run.sh /run.sh
  - Adds **the contents** of tar archieves: ADD project.tar.gz /install/
  - Works with URLs: ADD https://project.example.com/download/1.0/project.rpm /project/
- ENV statement:
  - Set environment variables
  - Both during the build and when running the result:
    - ENV DB_HOST=db.production.example.com
    - ENV DB_PORT=5432
- ENTRYPOINT RUN and CMD statements:
  - They can use either form:
    - Shell form: nano notes.txt
    - Exec form: ["/bin/nano", "notes.txt"]
- EXPOSE statement:
  - Maps a port into the container
    - EXPOSE 8080
- VOLUME statement:
  - Define shared or ephemeral volumes:
    - VOLUME ["/host/path/" "/container/path/"]
    - VOLUME ["shared-data"]
  - Avoid defining shared folders in Dockerfiles
- WORKDIR statement
  - Sets the directory when container starts
  - Similar to cd
    - WORKDIR /install/
- USER statement
  - Sets which user the container will run as
    - USER arthur
    - USER 1000

## Multiproject Dockerfiles:
- Image can be big
- We can have 2 FROM in one dockerfile, the first one as the builder to give the result we want, then then second use the result of the first one. By that way, the result can be big.
- Example:
  - This Dockerfile generates a big image as it is downloading the whole Ubuntu

```Docker
FROM ubuntu as builder
RUN apt-get -y update
RUN apt-get -y install curl
RUN curl https://google.com | wc -c > google_size
ENTRYPOINT echo google is this big; cat google_size
```
  - We can have 2 FROM and the result using the small alpine, thus the final image is small, which is only 5.6MB
```Docker
FROM ubuntu as builder
RUN apt-get -y update
RUN apt-get -y install curl
RUN curl https://google.com | wc -c > google_size

FROM alpine
COPY --from=builder /google_size /google_size
ENTRYPOINT echo google is this big; cat google_size
```
![image](https://user-images.githubusercontent.com/79841341/129467931-5bbef482-58c9-4aef-a8e5-9d0c57a77f7f.png)

# Orchestration: Building Systems with Docker

