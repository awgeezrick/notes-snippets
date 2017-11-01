# Docker - notes and useful commands
------------------------

# Contents

## Docker Set Up
- Docker doesn't run natively on Mac or Windows. Small VM is started and run in background to run Docker
- Docker has versions for cloud providers such as AWS and Azure and comes with important features to manage resources on those platforms


## CLI

```sh
# Syntax
docker [mgmt-cmd] [sub-cmd] (options)

docker version
# Gives client and server information
# Also validates that you can talk to the server
docker info
# Gives much more information
docker
# Gives list of management cmds and some sub-cmds
```
# Container Basics
- Image - an application we want to run
- Container - an instance of the image running as a process
  - Many containers can run off the same image
- Dockers default image registry is hub.docker.com

```sh
docker container run --publish 80:80 nginx
# Starts an Nginx web server
docker container run --publish 80:80 --detach nginx
# Runs Nginx server in the background
#     Frees up the terminal prompt

# Changing defaults...
docker container run --publish [listen-port-nmbr]:80 \
  --detach --name [container-name] -d nginx:[img-v-nmbr] [CMD-run-on-start] -T
# Allows you to...
    # change port numbers
    # specify a name for a container
        # Otherwise name is assigned from randomly generated lists
    # specify image versions
    # change CMD run on start up

docker container logs [container-name]
# Shows log history that was otherwise hidden by --detach
docker container top [container-name]
# Lists process running inside container
docker container ls
# Lists running containers
docker container ls -a
# Lists all container images, even those not running
docker container stop [container-ID]
# Stops running container
docker container --help
# lists commands we can run on the container
docker container rm [ID1] [ID2] [...]
# Removes containers - will not remove running containers
docker container rm -f [ID]
# Force removes running container

```
- Looks for image locally in image cache
- If not local, looks in remote image repository (defaul is Docker Hub)
- Downloads latest version (unless version# is specified)  
- Creates new container based on image and prepares to started
- Gives container a virtual IP on private network inside docker engine
- Opens up port we specified on host and forwards to port we specified in the container
- Starts container by using the CMD in the image Dockerfile
