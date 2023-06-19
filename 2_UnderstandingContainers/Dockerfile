# We will use nginx:alpine as our base image.
FROM nginx:alpine
# Replace the default nginx html file with our own.
COPY index.html /usr/share/nginx/html
# Exposing port 80 we instruct that the container will listen on host port 80.
EXPOSE 80