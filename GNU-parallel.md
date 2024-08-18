
1. Dockerfile:

```Dockerfile 
FROM ubuntu:latest
ENV DEBIAN_FRONTEND=noninteractive

# Install required packages
RUN apt-get update && \
    apt-get install -y wget build-essential && \
    wget http://ftpmirror.gnu.org/parallel/parallel-latest.tar.bz2 && \
    tar -xjf parallel-latest.tar.bz2 && \
    cd parallel-* && \
    ./configure && \
    make && \
    make install && \
    cd .. && \
    rm -rf parallel-* parallel-latest.tar.bz2 && \
    apt-get remove --purge -y wget build-essential && \
    apt-get autoremove -y && \
    apt-get clean

# Set the default command to run GNU Parallel
CMD ["parallel", "--version"]
```
2. Build the Docker Image:
```sh
docker build -t alhumaidyaroob/gnu-parallel:latest .
```


4. Test the Docker Image
    - Add your dockerhub user instead of alhumaidyaroob
```sh 
docker run --rm alhumaidyaroob/gnu-parallel:latest --version
```

5. Log in to Docker Hub
    - Before you can push the Docker image to Docker Hub, you need to log in:
    - Enter your Docker Hub username and password.
```sh
docker login
```

6. Push the Docker Image to Docker Hub
    - Add your dockerhub user instead of alhumaidyaroob
```sh
docker push alhumaidyaroob/gnu-parallel:latest
```

7. Use the Docker Image
    - To pull the image:
```sh
docker pull alhumaidyaroob/gnu-parallel:latest
```
    - To run the image from docker hub: 
```sh
docker run --rm -v $(pwd):/data  -w /data
```