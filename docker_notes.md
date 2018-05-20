# Docker - notes and useful commands
------------------------

# Contents
- [Docker Set Up](#docker-set-up)
- [Resources](#resources)
- [CLI](#cli)
- [Container Basics](#container-basics)
- [Inspecting Containers](#inspecting-containers)
- [Images](#docker-images)
- [Dockerfile Basics](#dockerfile)
- [Data Science with Docker](#data-science-with-docker)

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
#### Docker Bash Completion
```sh
# To set up bash completion for Docker...
#   1. Make certain bash-completion is installed...
brew install bash-completion
#   2. Add the following yo .bash_profile ...
if [ -f $(brew --prefix)/etc/bash_completion ]; then
. $(brew --prefix)/etc/bash_completion
fi
#   3. Create symlinks to add docker-specific completions...
ln -s /Applications/Docker.app/Contents/Resources/etc/docker.bash-completion \     
      /usr/local/etc/bash_completion.d/docker
ln -s /Applications/Docker.app/Contents/Resources/etc/docker-machine.bash-completion \
      /usr/local/etc/bash_completion.d/docker-machine
ln -s /Applications/Docker.app/Contents/Resources/etc/docker-compose.bash-completion \
      /usr/local/etc/bash_completion.d/docker-compose
# Restart bash and docker bash completion should now be working.
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
Documentation for `docker container run`: https://docs.docker.com/engine/reference/commandline/container_run/
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
# To remove containers automatically when stopped...
--rm # ...add this option to the docker container run command.
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

### Inspecting Containers
```sh
docker container top [container-name]
# ...Process list for container
docker container inspect [container-name]
# ...Details of container config
docker container stats
# ...performance stats for all containers
```

#### Getting a shell inside containers
```sh
# NO SSH NEEDED
-it # option
    # -t allocates a pseudo TTY
    # -i keeps STDIN open even if not attached
docker container run -it [command]
# ...starts a new container interactively
docker container exec -it [command]
# ...allows you to run commands in a running container

# EXAMPLES...
docker container run -it --name [container-name] [container-type] bash
# ...starts new container and takes you to bash prompt as root
# ...this assumes the container already contains bash
# ...if not it will need to be added
exit # typing this in bash prompt exits bash and stops container

docker container exec -it --name [container-name] [container-type] bash
# ...opens bash prompt for already running container
exit # exits bash, but does affect root process for running container

docker container run -it ubuntu bash
# ...this installs full ubuntu instance as container and gives you prompt
# this is a very minimal version of ubuntu, but can be added to with apt-get...
apt-get [package-name] # can add any additional tools to install
```
# Attach Working Directory as Volume (shared filesystem) in Docker Container
```sh
# This is accomplished using the -v volume command

docker run -it -v /$PWD:/home/directory ubuntu
```

# Docker Image Basics
```sh
docker pull [image-name]
# ..docker pulls down latest version of the image
docker image ls
# ...the disk size and additional details for all local images
```
Different linux distros in containers

- Linux Alpine is <4MB in size and contains `sh` not `bash` as its shell

# Docker Networks

Docker Networks defaults
- Each container is connected to a private virtual network bridge
- Each routes through NAT firewall on host IP
- All containers on a virtual network can talk to each other without -p
- Best practice is to create a new virt network for each application. e.g.:
  - web app network for mysql and php/apache containers
  - api network for mongo and nodejs containers
- "Batteries included, but removable"
- Can make new virtual Networks
- Can have mult. networks on one container
- Can skip virt networks and use host host IP (`--net=host`)
- Use entirely different Docker network drivers based on needed capabilities

```sh
docker container run -p [host:container]
# ...(--publish) option exposes the port of container
docker container port [container-name]
# ...Returns the ports fowarding traffic to the container from the host
docker container inspect --format '{{[template-parsing-param]}}' [container-name]
# ...--format is a common option for formatting
#       the output of commands using "go templates"

```

### Docker Networks - CLI Management
```sh
docker network ls
# ...shows Networks
docker network inspect [network-name]
# ...inspects a network
docker network create --driver
# ...creates network using 3rd party driver
docker network connect [network-name] [container-name]
# ...attach a network to a container
docker network disconnect [network-name] [container-name]
# ...detaches a network from a container

```
Standard three docker networks
1. bridge (docker0) - Default virtual network. Is NAT'ed behind the host IP
2. host - Special network that skips the virtual networking of docker and attaches container directly to host interface
3. none - removes eth0 and leaves only the localhost interface in the container

### Docker Networks - DNS

- Forget IPs.
  - Static IPs and using IPs for talking to containers is an anti-pattern. Do your best to avoid it. IPs are too dynamic in the world of docker and micro-services
- Docker DNS.
  - Docker daemon has a built-in DNS server that containers use by default.
  - Docker defaults the hostname to the container's name.
  - You can also set aliases


## Docker Images
Definition: An image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime.

image layers


union file system
`history` and `inspect` commands
copy on write

```sh
docker image ls
# ...lists all images on local system
#   REPOSITORY    TAG   IMAGE ID    CREATED   SIZE

docker image history [repository:tag]
# ...shows layers of changes made in image: history of image layers
# starts with base layer named "scratch" and shows each change
# Allows us to never save the same image data on the same system
#   instead, we are saving the differencing between versions
#   therefore a container is just a single read/write layer
#       on top of an image

# defaults to "latest" if no tag is specified

docker image inspect [repository:tag]
# ...gives you back the metadata about the image
#     and how it expects to run

docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
# ...assigns one or more tags to an image
# Should be named "personal-repository/image-name:tag-name"
#   to ensure you can push to your docker hub repo

# Log-in and log-out from Docker Hub account via CLI...
docker login
docker logout
# ...stores session ID on machine, so logout from shared machine

docker image push SOURCE_IMAGE[:TAG]
# ...pushes docker image to personal repository
```
### Dockerfile

```sh
# To build docker image from active directory Dockerfile...
docker image build -t [custom-name] .

# FIVE BASIC STANZAS FOR DOCKERFILE (FROM, ENV, RUN, EXPOSE, CMD)

# BEST PRACTICE:
#   - Put things that change the most at the bottom of your
#       Dockerfile
#   - This is because of docker caching practices and rebuilding
#       images that have been changed happens in order of lines as
#       they appear in the Dockerfile

FROM linux-distro:version
# ...all images must have a FROM statement (usually a minimal
#     linux distro such as 'alpine')
WORKDIR /some/path
# ...changes the working directory to root of container
#   this is preferred to using "RUN cd /some/path"
ENV NGINX_VERSION 1.13.6-1~stretch
# ...optional environment variable used in Dockerfile and set as
#     envvar when the container is running
RUN [shell-command] && [shell-command] && ...
# ...runs shell commands during setup of container
# ...&& ensures all changes are all placed in one layer

# RUN can also be used to forward request and error logs
#     to the docker log collector...e.g....
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf dev/stderr /var/log/nginx/error.log

EXPOSE 80 443
# ...exposes these ports on the docker virt. network, but still need #      to use -p or -P to open/forward these ports on host

COPY [filename] [filename]
# ...copies file from root directory and overwrites with out custom
#     file, such as example of overwriting NGINX default index.html
#     with a customer index.html

CMD []
# ...required parameter that is final command run every time a new
#     container is launched from the image
# ...every image inherits any CMD stanza from a parent image, so
#     depending on the source of a dockerfile, this may or may not
#     be needed.
```

### Data Science with Docker
I am still in search of the best workflow for data analysis in Python and R using Jupyter Notebooks with Docker containers.

Currently, the most straight forward approach is building and running one of the Jupyter Project's ready-made Docker stacks: <https://github.com/jupyter/docker-stacks>

Jupyter's "datascience-notebook" stack is the most complete for my uses (R and Python mixed analysis in Jupyter), however it is far too large of an image (it includes Julia and a number of R and Python packages I will likely not need to use).

Example provided below. See Jupyter's GitHub repo for more specific directions.

```sh

# Insert path to local host directory I wish to use as my Jupyter working directory.

docker run -it --rm -p 8888:8888 -v ~/path/to/preferred/active/directory:/home/jovyan/work \
  jupyter/datascience-notebook
```
