# DOCKER TIPS

## docker
> Install docker & docker-compose on Ubuntu 20.04
> Docker have deprecated their old versions
> Add their repo and re-install

```
sudo apt-get remove docker docker-engine docker.io containerd runc
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository  "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get install docker-ce docker-ce-cli containerd.io
```
You will need to add your $USER to the **docker** group.
```
sudo groupadd docker
sudo usermod -a -G docker $USER
sudo systemctl restart docker.service
```
It is recommended that you logout and login again (or reboot) as well.

If you need to login to hub.docker.com to access a private repo: 
```
docker login -u bankbuilder -p *****
```


### Docker Cheat Sheet

```
docker pull alpine:3.4

# or most often

docker pull alpine:latest

```
Break down of the docker run command:
```
docker run
          --rn  remove container automatically after it exits
          --it  connect interactively to the terminal
   -- name web  name the container 'web'
    -p 5000:80  expose the internal port 80 as port 5000 externally
 -v ~/dev:/src  map local folder ~/dev into the container at /src
 alpine:latest  the image:version to be instantiated
        bin/sh  the entry point command to run inside container
```

or simply:
```
docker run -it --name web -p 5000:80 -v ~/dev:/src alpine bin/sh
```

To stop the container with SIGTERM:
```
docker stop web
```

To stop the container with SIGKILL:
```
docker kill web
```

Docker uses the default 172.17.0.0/16 subnet for container networking.

#  To list the docker networks
```bash
docker network ls
```

# To inspect the metadata about the network

```bash
docker network inspect <network-name>
```

# Killer list of useful commands
```
docker ps                        # List running containers
docker ps -a                     # List running & stopped containers
docker stats                     # Display a live stream of container(s) resource usage statistics
docker top {container}           # Display the running processes of a container
docker stop $(docker ps -q)      # Stop running containers
docker rm $(docker ps -q)        # Remove stopped containers
docker rm -f $(docker ps -aq)    # Hardcore delete all running & stopped container instances
docker images                    # Neat list of all docker images
docker image list		     # Long hand version of the above
docker image inspect {container} # Display detailed information on one or more images
docker image prune               # Removes all dangling containers, usually as a result of docker builds

docker image rm alpine:3.4       # Removes the actual container image from your local system
docker system prune               # Removes all unused docker data - dangling containers, networks, images and volumes
```
So when you use a volume in a docker-compose or docker container it then create a volume on your local machine.
These local docker volumes start to dangle after a while taking up space on your drive. 
This will help:
```
docker volume ls # will tell you what volumes exist
docker volume ls -f dangling=true # will tell you which may be safely deleted

docker rm $(docker ps -a -q)

docker volume rm $(docker volume ls -f dangling=true -q) # and delete the lot ...
```

You may just want to run a known process in a container:
```
docker exec -it web /bin/sh
```

and it is often useful to tail the container's logs:
```
docker logs --tail 100 web
```
## Docker-Compose

Start by uninstalling your existing version.

```bash
# If installed via package manager
sudo apt-get remove docker-compose

# If installed via curl
sudo rm /usr/local/bin/docker-compose

# If installed via pip
pip uninstall docker-compose

DESTINATION=/usr/local/bin/docker-compose

VERSION=$(curl --silent https://api.github.com/repos/docker/compose/releases/latest | jq .name -r)

sudo curl -L "https://github.com/docker/compose/releases/download/$VERSION/docker-compose-$(uname -s)-$(uname -m)" -o $DESTINATION
sudo chmod 755 $DESTINATION
```


When grappling with build compose files this has been useful
```
# Use this approach to rebuild 
docker-compose build --force-rm
```

[Home](./README.md) [Prev](./introduction.md) [Next](./git.md)

---
DevOps Enablement, Cyber-Mint (Pty) Ltd.     
[www.cyber-mint.com](https://www.cyber-mint.com)

