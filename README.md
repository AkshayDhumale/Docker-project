# Docker-project

Steps to download & run the project

DOWNLOAD:

1. Download and install Docker into you Machine (linux/mac or windonws) from official website
 # link : 
    https://www.docker.com/products/docker-desktop/
 # pull the code from Repository into you local machine
    git pull
2. All Instruction to create a Docker image is in the hello-node/Dockerfile

3. Also The docker-compose.yml file has the code to run the docker-compose

HOW TO RUN:

1. After Installing Docker RUN the below commands.
  # Build the Docker image
    docker build -t hello-node .

  # Run the Docker container, mapping port 3000 of the container to port 3000 on the host
    docker run -p 3000:3000 -d hello-node
    
2. To RUN docker-compose
  # Run the docker-compose
    docker-compose up

3. Access the API in browser
   # access the API by navigating to
       http://localhost:3000
