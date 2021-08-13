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

# Exposing ports:

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

## Volumes

It is used to sharing data between containers or containers and host. Volumes are not part of images, and can be:
- Persistent: Still exist after containers are closed.
- Ephemeral: Disappear when containers are gone.

Sharing data with the Host: It is persistent. ***-v*** option is used to achieve this goal.

```docker
docker run --rm -ti -v HOST_FOLDER:CONTAINER_FOLDER --name echo-server ubuntu20
```

![image](https://user-images.githubusercontent.com/79841341/129377882-29da44c8-87f0-4ba0-ad31-afbc07664092.png)

