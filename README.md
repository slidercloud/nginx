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
    
Browsing to http://<ip>:8000/ should render the following HTML page.
![Browser tab safari](https://i0.wp.com/camo.githubusercontent.com/a5808baa641f0c1257a75cd5013df8141ff220f5cb64eb0dcb1195878123ed79/68747470733a2f2f73332e75732d776573742d322e616d617a6f6e6177732e636f6d2f7365637572652e6e6f74696f6e2d7374617469632e636f6d2f35353538353536612d353838632d343637632d383135612d6435396335393637376661302f53637265656e5f53686f745f323032322d30322d31365f61745f32312e34352e30372e706e673f582d416d7a2d416c676f726974686d3d415753342d484d41432d53484132353626582d416d7a2d436f6e74656e742d5368613235363d554e5349474e45442d5041594c4f414426582d416d7a2d43726564656e7469616c3d414b49415437334c324734354549505433583435253246323032323034313725324675732d776573742d322532467333253246617773345f7265717565737426582d416d7a2d446174653d3230323230343137543231313534325a26582d416d7a2d457870697265733d383634303026582d416d7a2d5369676e61747572653d3362663466643536663035326362363063353838366132373638663562303035333866316162663936376264396337656436363762393262333466383538353226582d416d7a2d5369676e6564486561646572733d686f737426726573706f6e73652d636f6e74656e742d646973706f736974696f6e3d66696c656e616d6525323025334425323253637265656e253235323053686f742532353230323032322d30322d313625323532306174253235323032312e34352e30372e706e6725323226782d69643d4765744f626a656374)

# Pricing for Cloutty Edge Delivery (1 Point of Presence)
| VM Pricing | Starter | Small Business |
|---|---|---|
| Memory | 1GB | 2GB |
| CPU Cores | 1vCPU | 1vCPU |
| Persistent Storage | 20GB HDD | 40GB HDD |
| Bandwidth | $0,04 per GB | $0,04 per GB |
| Monthly pricing | $10 | $20 |
| Hourly pricing | $0,014 | $0,027 |

*Note that pricing is based on usage. Right now, the Service is only available to invited beta testers and select partners.
