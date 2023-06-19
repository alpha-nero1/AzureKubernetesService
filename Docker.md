# Docker
Software framework to create containerised applications.

- To run a container you need an Image! it is a template/snapshot of an application.
- Usually starts with base image like an OS.

Example Dockerfile:
```
FROM debian:latest
RUN apt-get update && apt-get -y install curl
CMD ["sleep", "infinity"]
```
\
And to run:
```
docker build -t my-img .
# -t = tag name., . points to the current dir.
```
\
If you run the following you will see the image created.
```
docker images
# and further
docker images | grep my-img
```
\
Now you can run the container from the image:
```
docker run -d --name my-cont my-img
# -d = run the process in the background.
# --name = name of the container.
```
\
With the following we can execute commands on the containers behalf:
```
docker exec my-cont curl google.com -v
# -v = verbose, outputs details of execution.
```
\
See running containers with:
```
docker ps
```
\
You can stop the container with:
```
docker stop my-cont
# And delete the image with:
docker rm my-img
```

### Interactive shell terminal
To exec into a container you will need to run:
```
docker exec -it <exec> sh
# it = interactive
```

## Dockerhub
Registry which stores images, we need to store and manage images somewhere in order to retrieve and deploy them.