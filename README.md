# How to create an NGINX Server on Docker
The following documentation covers the workaround on how to get a Docker NGINX container started on one of our Cloud VMs.

## Install Docker on your (Ubuntu Server 21.10) VM
Paste the following commands into your machine's terminal to connect to your VM.

    $ sudo ssh <username>@<server-dc-id>

Once connected, we'll download Docker from the cloud.

If you already have an old version of Docker, uninstall it first. The contents of `/var/lib/docker/`, including images, containers, volumes, and networks, are preserved.

    $ sudo apt-get remove docker docker-engine docker.io containerd runc

It’s OK if `apt-get` reports that none of these packages are installed.

Next, setup your Docker repository.

    $ sudo apt-get update
    $ sudo apt-get install \
        ca-certificates \
        curl \
        gnupg \
        lsb-release
    
And add Docker's official GPG key. 

    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

Use the following command to set up the stable repository.

    $ echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
    $ (lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

*Sudo will prompt you to enter your admin password.

## Install Docker Engine

    $ sudo apt-get update
    $ sudo apt-get install docker-ce docker-ce-cli containerd.io
    
Verify that Docker Engine is installed correctly by running the `hello-world` image.

    $ sudo docker run hello-world
    
This command downloads a test image and runs it in a container. When the container runs, it prints a message and exits.

_____

# Create an image for NGINX
Now that Docker is installed and fully working. Clone this repository (requires Git and Docker Compose).

    $ git clone https://github.com/airtheo/docker-nginx.git
    $ cd docker-nginx
    $ sudo docker build -t docker-static . // builds your image
    // start your Docker container on port 8000 (Running locally on port 90 like specified in nginx.config)
    $ sudo docker run -p 8000:90 docker-static
