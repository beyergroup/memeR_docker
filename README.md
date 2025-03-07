# memeR_docker
This repository includes the Dockerfile for building a Docker image to run the meme suite inside R/RStudio, along with a brief tutorial on how to use it. This tutorial is for the use case if you want to run an RStudio server on your workstation, which you can access in addition to your usual RStudio server, because you will have the right to install other tools if needed.

## Step 1: Get the Dockerfile
Clone the repository or download the Dockerfile.
```{bash}
git clone https://github.com/beyergroup/memeR_docker.git
```

## Step 2: Building the Docker image 
Go to the directory of the Dockerfile on your system and build the image.
```{bash}
docker build --tag memeR .
```

## Step 3: Running the container 
I would recommend running the Docker container like this:
```{bash}
docker run -d -e PASSWORD=pass -e USERID=$(id -u) -e GROUPID=$(id -g) -e ROOT=TRUE -p 9000:8787 -v .:/data memeR
```
Explanation:
- `⁠-d`: We detach from the container so it runs on the workstation until we stop it.
- `⁠-e`: These are environment variables for the RStudio base image, e.g., PASSWORD (which you can change), etc. (I would recommend using the ROOT=TRUE variable so you as the RStudio user have rights to install other software you need).
- `-p`: This is for routing the ports.
    - The first port can be changed to whatever you need (8787 is probably already taken by your regular RStudio server on your workstation).
	- The port after the ⁠: is for inside the container, which has to be 8787 because this is what RStudio uses.
- `⁠-v`: This is for mounting a volume from your workstation. You can give it a path or just use the dot, which mounts your current directory.
	- Again, before the ⁠: is for the path on your machine, and after is for the path inside the Docker container.
## Step 4 (optional): SSH tunneling to access the RStudio server on your personal machine
Lastly, if you want to connect to the server on your machine, you need to use port tunneling because not all the ports of the workstations are accessible.
In your terminal, run:
```{bash}
ssh -L 9000:127.0.0.1:9000 <your regular worstation ssh command>
```
I used 9000 because this is the port I used for running the Docker on my workstation.
Now you can open your browser on your machine and type in the URL bar:

```{bash}
http://localhost:9000/
```
