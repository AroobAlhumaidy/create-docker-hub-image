# create-docker-hub-image
This repository is used to point out the steps you need to do if you want to transfer a tool to dockr-hub 

# sambama tool: 
1. Clone the Sambamba Repository
```sh
git clone https://github.com/biod/sambamba.git
cd sambamba
```

2. Create a Dockerfile
Inside the Sambamba directory, create a Dockerfile. This file will define the environment and instructions for building the Sambamba tool.
```Dockerfile
FROM ubuntu:20.04
ENV DEBIAN_FRONTEND=noninteractive

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    git \
    curl \
    liblz4-dev \
    zlib1g-dev \
    libbz2-dev \
    libssl-dev \
    ldc \
    dub \
    && rm -rf /var/lib/apt/lists/*

# Clone the Sambamba repository
RUN git clone https://github.com/biod/sambamba.git /sambamba

# Set the working directory
WORKDIR /sambamba

# Compile Sambamba using the Makefile
RUN make sambamba-ldmd2

# Add Sambamba to PATH
ENV PATH="/sambamba/bin:$PATH"

# Set the entrypoint to Sambamba
ENTRYPOINT ["sambamba"]

# Default command to display the version
CMD ["--version"]

```

3. Build the Docker Image
    - Add your dockerhub user instead of alhumaidyaroob
    - if you have versions, add it instead of latest 
```sh
docker build -t alhumaidyaroob/sambamba:latest .
```

4. Test the Docker Image
    - Add your dockerhub user instead of alhumaidyaroob
```sh 
docker run --rm alhumaidyaroob/sambamba:latest --version
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
docker push your_dockerhub_username/sambamba:latest
```

7. Use the Docker Image
    - To pull the image:
```sh
docker pull your_dockerhub_username/sambamba:latest
```
    - To run the image from docker hub: 
```sh
docker run --rm -v $(pwd):/data  -w /data
```