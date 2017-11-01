# Docker - notes and useful commands
------------------------

# Contents
- [Docker Set Up](#docker-set-up)
- [Resources](#resources)
- [CLI](#cli)
- [Container Basics](#container-basics)
- [](#)

## Docker Set Up
- Docker doesn't run natively on Mac or Windows. Small VM is started and run in background to run Docker
- Docker has versions for cloud providers such as AWS and Azure and comes with important features to manage resources on those platforms

## Resources

- [docs.docker.com](#docs.docker.com)
- `docker [command] --help`

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
--env # (or -e is the environment var allowing
      #   us to pass env-based settings into container)

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
#### Sample code for running, then clearing multiple containers
```sh
# Run nginx container...
docker container run -d --name nginx -p 80:80 nginx
# Run httpd(apache) container...
docker container run -d --name httpd -p 8080:80 httpd
# Run mysql container...
docker container run -d --name mysql -p 3306:3306 \
  -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql
# Check mysql log for generated root password needed to log
#   into mysql server...
docker container logs mysql
# Check they are all running...
docker container ls
# Test server ports active...
curl localhost && curl localhost:8080
# Stop containers...
# Remove containers...
docker container stop mysql nginx httpd
docker container rm mysql nginx httpd
# Confirm removed...
docker container ls -a
```
#### Definitions
- Image - an application we want to run
- Container - an instance of the image running as a process
  - Many containers can run off the same image

#### When running a docker image, docker...
1. Docker looks for image locally in image cache
1. If not local, looks in remote image repository (default is [hub.docker.com](#hub.docker.com))
1. Downloads latest version (unless version# is specified)  
1. Creates new container based on image and prepares to started
1. Gives container a virtual IP on private network inside docker engine
1. Opens up port we specified on host and forwards to port we specified in the container
1. Starts container by using the CMD in the image Dockerfile

#### Containers vs VMs

- Containers are not mini-VMs
- Containers are just processes that:
  - Have a limited number of resources they can access
  - Are exited when the process stops
- When running a container bash command `ps aux` or `ps aux | grep [container-name]` shows all processes running on system
  - Container is listed, but with a different PID
