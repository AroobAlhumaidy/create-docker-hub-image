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
    && rm -rf /var/lib/apt/lists/*

RUN curl -fsS https://dlang.org/install.sh | bash -s dmd

# Clone the Sambamba repository
RUN git clone https://github.com/biod/sambamba.git /sambamba

# Set the working directory
WORKDIR /sambamba

# Compile Sambamba
RUN ./build.sh

# Add Sambamba to PATH
ENV PATH="/sambamba:$PATH"

# Set the entrypoint to Sambamba
ENTRYPOINT ["sambamba"]

# Default command to display the version
CMD ["--version"]
```

