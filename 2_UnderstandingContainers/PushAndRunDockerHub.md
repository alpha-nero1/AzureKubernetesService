As a precursor to all of this you may need to run and follow:
```
docker login
```

docker run -d nginx:alpine #run nginx:alpine image

builds a Docker image based on the Dockerfile in the current directory and tags the image with the name "my-simple-website":
`docker build -t my-simple-website .`

tags the existing image my-simple-website with a new repository name <your-registry>/my-simple-website and a tag name v1. We will use this to push to Docker:
`docker tag my-simple-website alphanero1/my-simple-website:v1`

pushes the image to the registry (note that my-simple-website image must exist locally.):
`docker push alphanero1/my-simple-website:v1` #

starts a new container from the image we just pushed to Docker. The -p option maps a host port to a container port. In this command, the host port 80 is being mapped to the container port 80, meaning that incoming traffic on the host's port 80 will be forwarded to the container's port 80. This allows us to access the container's application that is listening on port 80 from the host by accessing http://localhost:80 in the web browser:
`docker run -p 80:80 alphanero1/my-simple-website:v1`