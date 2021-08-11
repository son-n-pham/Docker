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

![image](https://user-images.githubusercontent.com/79841341/129069980-12ec3ef5-ba11-4ae5-b042-ec06f010edb0.png)

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

## Container Network

