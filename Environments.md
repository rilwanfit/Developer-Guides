---
id: env
title: Working Environment
---

## Working with Mysql

### Check MySQL service running or not

```
service mysql status
```

### Start / Stop MySQL service
```
service mysql start
service mysql stop
```

### Login 

```
mysql -u root -p
```

1.  Create new user

```
create user 'testuser'@localhost identified by 'password';
```

2.  List existing users

```
SELECT User FROM mysql.user;
```
3.  Grant permission to user
```
grant all on testdb.* to 'testuser';
grant all on testdb.* to 'testuser' identified by 'password';
```
4.  Exit MySQL

```
exit
```

### Reset Root Password

1.  Stop current service instance

```
sudo service mysql stop
```

2.  Use **dpkg** to rerun the configuration process MySQL goes through on first installation. You will again be asked to set a root password.

```
sudo dpkg-reconfigure mysql-server-5.5
```

3.  Start the service
```
sudo service mysql start
```

# HHVM

[https://3v4l.org/drJCV](https://3v4l.org/drJCV)


## Docker

## Docker vs Linux Container

Containers are similar to a virtual machine, because they allow you to run different operating systems and applications, and separate them from the host machine. Unlike virtual machines, containers don't have the overhead of an entire operating system.

Containers work by sharing the kernel of the host machine that they're running on, while allowing complete isolation of processes.  this is what allows you to run two different version of PHP on the same machine at once.

Having kernel support and container functionality is one thing, but making it easy to use is another.  is is where Docker excels.	

Docker can help to:

*   Emulate production in Development, Test, and Stage.
*   Onboard new developers.
*   Support multiple versions and simplify switching between them.
*   Simplify your local development setup.
*   Better utilize local system resources.

[https://docs.docker.com/](https://docs.docker.com/)

[https://www.goetas.com/blog/how-do-i-deploy-my-symfony-api-part-1-development/](https://www.goetas.com/blog/how-do-i-deploy-my-symfony-api-part-1-development/)

[https://www.jverdeyen.be/docker/how-php-symfony-coreos-docker/](https://www.jverdeyen.be/docker/how-php-symfony-coreos-docker/)

[http://www.newmediacampaigns.com/blog/docker-for-php-developers](http://www.newmediacampaigns.com/blog/docker-for-php-developers)

[http://blog.humblyarrogant.io/post/2017-01-31-Increase-docker-device-size/](http://blog.humblyarrogant.io/post/2017-01-31-Increase-docker-device-size/)

A lot of emphasis was used to ensure a stateless approach in each component. From the docker point of view this meant that was not possible to use shared folders between container and the host to store the state of the application (sessions, logs, ...).

Not storing data on the host is fundamental to be able to deploy seamlessly the application to live. Logs, sessions, uploads, userdata have to be stored in specialized services as log management tools, key value storages, object storages (as AWS S3 or similar).

Why ?



*   Allowing to easily generate development environments for any kind of application.


## Basics 

[http://training.play-with-docker.com/beginner-linux/](http://training.play-with-docker.com/beginner-linux/)


## Docker Enterprise edition

Enterprise development and teams that run business-critical containerized applications and services in production at large scales.

Provides a support mechanism for issues with the underlying engine as well as deployment challenges using it.

When using Docker EE on a certified platform, organizations are assured through Docker's certification that their applications will work as expected and have support available if they do not.

EE comes in how many different levels?

Three. Basic, Standard, and Advanced tiers.

[https://docs.docker.com/ee/supported-platforms/#docker-ee-tiers](https://docs.docker.com/ee/supported-platforms/#docker-ee-tiers)


## Docker Installation

*   CentOS - RedHat 
*   Debian / Ubunto

[https://docs.docker.com/install/linux/docker-ce/centos/#install-using-the-repository](https://docs.docker.com/install/linux/docker-ce/centos/#install-using-the-repository)


### Selecting a storage driver
[https://docs.docker.com/storage/storagedriver/select-storage-driver/#check-your-current-storage-driver](https://docs.docker.com/storage/storagedriver/select-storage-driver/#check-your-current-storage-driver)


### Configuring Logging Drivers
[https://docs.docker.com/config/containers/logging/configure/](https://docs.docker.com/config/containers/logging/configure/)

#### Docker service

[https://docs.docker.com/edge/engine/reference/commandline/service/](https://docs.docker.com/edge/engine/reference/commandline/service/)

Outline the Sizing requirements prior to installations:

[https://docs.docker.com/ee/ucp/admin/install/system-requirements/#enable-esp-traffic](https://docs.docker.com/ee/ucp/admin/install/system-requirements/#enable-esp-traffic)


### Set Up and Configure Universal Control Plane (UCP) and Docker Trusted Repository (DTR) for Secure Cluster Management

*   Minimum 2G Memory required
*   Make sure cluster created. **_Docker node ls _**shows results if the cluster created and nodes added to the cluster
*   Install UCP on manager

    **_Docker container run --rm -it --name ucp -v /var/run/docker.sock:/var/run/docker.sock docker/ucp:tag install --host-address 172.31.116.158 --interractive_**


    -v options indicates that we link /var/run/docker.sock with /var/run/docker.sock inside our container

*   


## Kubernetes

**What?** Open source container orchestration tool

[https://linuxacademy.com/cp/courses/lesson/course/2303/lesson/6/module/217](https://linuxacademy.com/cp/courses/lesson/course/2303/lesson/6/module/217)



1.  Configure kubernetes master.

**_sudo kubeadm init --pod-wetwork-cidr=10.244.0.0./16 _**

Flannel network pre-configured to use **_10.244.0.0./16_**

Above command help us to get access to**_ kubectl_**

Setup the user to use kubectl, instruction available at the completion of above command.



1.  Setup pod networking

Sudo kubectl apply -f [https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml](https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml)

**_Kubectl get pods_**

**_Kubectl get pods --all-namespaces_**


## Docker Images

We can search images on hub: `docker search name_of_the_image`

Install images from hub: `docker pull name_of_the_image`

Inspect image : `docker inspect name_of_the_image:tag`

We can list images this way: `docker image ls`

Get the list of image Ids: `docker image ls -q`

Remove all the images: `docker image rm $(docker image ls -q)` or  **_docker image rm `docker image ls -q`_**

> An image is like a Class. A Container is an instance of an image, just like an Object is an instance of some Class.

## Create a docker container

Create a container to run it later on with required image. 
````
docker create --name <container-name> <image-name>
````

#### Running a Container
```
docker run --rm \
    -d \
    -v $(pwd):/application:/var/www/html \
    -p 8080:80 \
    shippingdocker/phpapp
```

Start a container: `docker start container_id`

Stop running container: `docker stop container_id`

Remove all the images that have running containers in the past: 
```
docker image rm `docker ps ls -q`
```

> Force Removing a image that have running container will not affect the running containers because the running container have full snapshot of the required image.


#### Container lifecycle



1.  Run a container **_docker run --help _**for more details of options
1.  Attach to running container: **_docker attach container_name_**


#### [https://medium.com/devopsion/life-and-death-of-a-container-146dfc62f808](https://medium.com/devopsion/life-and-death-of-a-container-146dfc62f808)


### Port and volume

The Dockerfile instruction **EXPOSE**, the Docker run options **-p and -P**, and Docker Compose **expose and ports** all specify how to connect Docker containers over Docker networks and to host machines.

Find out the IP Address assigned to a container: **_docker inspect container_id | grep IPAddr_**


#### Docker port

To view a list of the ports defined on a container: 

**_docker port CONTAINER [PRIVATE_PORT[/PROTOCOL]]_**


#### Dockerfile EXPOSE vs publish


##### EXPOSE

When writing your Dockerfiles, the instruction **_EXPOSE_** tells Docker the running container listens on specific network ports. This acts as a kind of port mapping documentation that can then be used when publishing the ports.

You can also specify this within a docker run command, such as: **_docker run --expose=1234 my_app_**

But EXPOSE will not allow communication via the defined ports to containers outside of the same network or to the host machine. To allow this to happen you need to publish the ports.


##### Publish ports and map them to the host

There are several flags you can use when using the docker run command to publish a container's ports outside of the container's network and map them the host machine's ports. These are the **flags -p and -P**, and they differ in terms of whether you want to publish one or all ports.

To actually publish the port when running the container, use the -p flag on docker run to publish and map one or more ports, or the -P flag to publish all exposed ports and map them to high-order ports.

**_docker run -p 80:80/tcp -p 80:80/udp my_app_**

In the above example, the first number following the -p flag is the host port, and the second is the container port.

To publish all the ports you define in your Dockerfile with EXPOSE and bind them to the host machine, you can use the -P flag.

**_docker run -P my_app_**


#### Docker Compose expose vs ports

When using Docker Compose to define containers, the docker-compose.yml uses the instructions expose and ports to expose and publish containers' ports.


##### expose

Like EXPOSE in a Dockerfile, this instruction is used to expose ports without publishing them to the host machine — they'll only be accessible to linked services on the same network.

expose:

 - "3000"

 - "8000"


##### ports

This is used to publish ports to a host. You can either use a short syntax, or give a more detailed configuration.

Either specify both ports (HOST:CONTAINER), or just the container port (an ephemeral host port is chosen).

ports:

 - "3000"

 - "8000:8000/tcp"

 - "127.0.0.1:8001:8001"

The long form syntax allows the configuration of additional fields that can't be expressed in the short form.

target: the port inside the container

published: the publicly exposed port

protocol: the port protocol (tcp or udp)

mode: host for publishing a host port on each node, or ingress for a swarm mode port to be load balanced.

ports:

  - target: 80

    published: 8080

    protocol: tcp

    mode: host

[https://medium.freecodecamp.org/expose-vs-publish-docker-port-commands-explained-simply-434593dbc9a3](https://medium.freecodecamp.org/expose-vs-publish-docker-port-commands-explained-simply-434593dbc9a3)

[https://linuxacademy.com/cp/courses/lesson/course/710/lesson/8/completed/7/module/86](https://linuxacademy.com/cp/courses/lesson/course/710/lesson/8/completed/7/module/86)


#### 
Docker Volumes

Traditionally a volume is a logical drive which is a storage area accessible within a file system and typically resident in a partition of a hard disk.

How to create volume?



1.  -v flag

**_Docker run -v volume-name:/app_** \


Second parameter is mapping directory. (/app)

 \
**_Docker volume inspect volumn-name_**



1.  --mount flag

**_Docker run --mount source=volume-name,target=/app_**



1.  **_Docker run --mount source=volume-name,destination=/usr/shared/..._**
1.  **_Docker run --mount source=volume-name,destination=/usr/shared.//, readonly_**


#### Removing Volumes


##### Remove one or more specific volumes - Docker 1.9 and later

Use the **_docker volume ls_** command to locate the volume name or names you wish to delete. Then you can remove one or more volumes with the docker volume rm command:

List: **_docker volume ls_** \
Remove: **_docker volume rm volume_name volume_name_**


##### Remove dangling volumes - Docker 1.9 and later

Since the point of volumes is to exist independent from containers, when a container is removed, a volume is not automatically removed at the same time. When a volume exists and is no longer connected to any containers, it's called a dangling volume. To locate them to confirm you want to remove them, you can use the docker volume ls command with a filter to limit the results to dangling volumes. When you're satisfied with the list, you can remove them all with docker volume prune:

List: docker volume ls -f dangling=true \
Remove: docker volume prune


#### 
Dockerfile


#### Purging All Unused or Dangling Images, Containers, Volumes, and Networks

Docker provides a single command that will clean up any resources — images, containers, volumes, and networks — that are dangling (not associated with a container):



*   docker system prune \


To additionally remove any stopped containers and all unused images (not just dangling images), add the -a flag to the command:



*   docker system prune -a


#### Removing Docker Images


##### Remove one or more specific images

Use the docker images command with the **-a flag** to locate the ID of the images you want to remove. This will show you every image, including intermediate image layers. When you've located the images you want to delete, you can pass their ID or tag to docker rmi:

List: **_docker images -a_**

Remove: **_docker rmi image_id or image_name:tag_**


##### Remove dangling images

Docker images consist of multiple layers. Dangling images are layers that have no relationship to any tagged images. They no longer serve a purpose and consume disk space. They can be located by adding the filter **flag, -f** with a value of **dangling=true** to the docker images command. When you're sure you want to delete them, you can use the docker images purge command:

Note: If you build an image without tagging it, the image will appear on the list of dangling images because it has no association with a tagged image. You can avoid this situation by [providing a tag](https://docs.docker.com/engine/reference/commandline/build/#/tag-image--t) when you build, and you can retroactively tag an images with the [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) command.

List: **_docker images -f dangling=true_**

Remove: **_docker images purge_**


##### Removing images according to a pattern

You can find all the images that match a pattern using a combination of docker images and [grep](https://www.digitalocean.com/community/tutorials/using-grep-regular-expressions-to-search-for-text-patterns-in-linux). Once you're satisfied, you can delete them by using **awk to pass the IDs to docker rmi**. Note that these utilities are not supplied by Docker and are not necessarily available on all systems:

List: docker images -a |  grep "pattern"

Remove: **_docker images -a | grep "pattern" | awk '{print $3}' | xargs docker rmi_**


##### Remove all images

All the Docker images on a system can be listed by adding -a to the docker images command. Once you're sure you want to delete them all, you can add the -q flag to pass the Image ID to docker rmi:

List: **_docker images -a_**

Remove: **_docker rmi $(docker images -a -q)_**


##### Removing Containers


##### Remove one or more specific containers

Use the docker ps command with the -a flag to locate the name or ID of the containers you want to remove:

List running containers: **_docker ps_**

List containers that i have run : **_docker ps -a_**

Remove container: **_docker rm ID_or_Name_**

Remove all the containers that have running in the past: **_docker rm `docker ps -a -q`_**


##### Remove a container upon exit

If you know when you're creating a container that you won't want to keep it around once you're done, you can run docker run --rm to automatically delete it when it exits.

**_docker run --rm image_name_**


##### Remove all exited containers

You can locate containers using docker ps -a and filter them by their status: created, restarting, running, paused, or exited. To review the list of exited containers, use the -f flag to filter based on status. When you've verified you want to remove those containers, using -q to pass the IDs to the docker rm command.

List: **_docker ps -a -f status=exited_**

Remove: **_docker rm $(docker ps -a -f status=exited -q)_** \



#### Remove containers using more than one filter

Docker filters can be combined by repeating the filter flag with an additional value. This results in a list of containers that meet either condition. For example, if you want to delete all containers marked as either Created (a state which can result when you run a container with an invalid command) or Exited, you can use two filters:

List: docker ps -a -f status=exited -f status=created \
Remove: docker rm $(docker ps -a -f status=exited -f status=created -q) \



### Remove containers according to a pattern

You can find all the containers that match a pattern using a combination of docker ps and [grep](https://www.digitalocean.com/community/tutorials/using-grep-regular-expressions-to-search-for-text-patterns-in-linux). When you're satisfied that you have the list you want to delete, you can use awk and xargs to supply the ID to docker rmi. Note that these utilities are not supplied by Docker and not necessarily available on all systems:

List:



*   docker ps -a |  grep "pattern" \


Remove:



*   docker ps -a | grep "pattern" | awk '{print $3}' | xargs docker rmi \



### Stop and remove all containers

You can review the containers on your system with docker ps. Adding the -a flag will show all containers. When you're sure you want to delete them, you can add the -q flag to supply the IDs to the docker stop and docker rm commands:

List:



*   docker ps -a \


Remove:



*   docker stop $(docker ps -a -q) \

*   docker rm $(docker ps -a -q) \



### Remove a container and its volume

If you created an unnamed volume, it can be deleted at the same time as the container with the -v flag. Note that this only works with unnamed volumes. When the container is successfully removed, its ID is displayed. Note that no reference is made to the removal of the volume. If it is unnamed, it is silently removed from the system. If it is named, it silently stays present.

Remove: docker rm -v container_name


#### Create nginx docker image



1.  Create Dockerfile with content of **FROM ubuntu:18:04**
1.  Start a new container from ubuntu and run bash. **_docker run --rm -it ubuntu:18.04 bash_**
1.  Inside the container, we can make some changes: **apt-get update && apt-get install -y nginx**
1.  Then we can inspect that container to see the changes:

	**# From outside the container**


    **docker diff <container-id-here>**



1.  We can then commit those changes to make a new image from that container and its changes!

    **docker commit -a "rilwan" -m "Nginx installed" <container-id-here> mynginx:latest**


Using docker file



1.  Create Dockerfile with content of [https://gist.github.com/rilwanfit/c7f02da8b6990261528e8adac3b092ae](https://gist.github.com/rilwanfit/c7f02da8b6990261528e8adac3b092ae)
1.  Build image from the Dockerfile

**docker build -t mhr/app:latest \**

**    -f docker/app/Dockerfile \**

**    docker/app # context**



1.  Start a new container and run bash **_docker run --rm -it _mhr/app_:_latest_ bash_**
1.  Check installed packages: **which nginx, which php**
1.  **Ps aux**
1.  Check the nginx settings: **_cd /etc/nginx/sites-available/ | cat default_**
1.  

Docker cleanup 

[https://gist.github.com/bastman/5b57ddb3c11942094f8d0a97d461b430](https://gist.github.com/bastman/5b57ddb3c11942094f8d0a97d461b430)

[https://stackoverflow.com/questions/21398087/how-can-i-delete-dockers-images](https://stackoverflow.com/questions/21398087/how-can-i-delete-dockers-images)


### Docker Compose

[https://docs.docker.com/compose/](https://docs.docker.com/compose/)

Docker compose is a tool that allows you to configure and coordinate containers.


### Start up order

Compose uses **depends_on**, links, volumes_from, and network_mode: "service: ..." to determine startup order.


### Compose files in public projects

[https://github.com/search?q=in%3Apath+docker-compose.yml+extension%3Ayml&type=Code](https://github.com/search?q=in%3Apath+docker-compose.yml+extension%3Ayml&type=Code)


### wait on another container's "ready" state,

you can use tools like [wait-for-it](https://github.com/vishnubob/wait-for-it) or [dockerize](https://github.com/jwilder/dockerize). These tools will poll the hosts and ports until TCP connections can be confirmed. You can append an entrypoint to your composition to force the wait:


```
services:
   web:
       entrypoint: "./wait-for-it.sh db:5432"
   db:
       image: postgres
```



### Compare versions

[https://docs.docker.com/compose/compose-file/compose-versioning/#version-3](https://docs.docker.com/compose/compose-file/compose-versioning/#version-3)


### Chaining multiple compose files

**docker-compose -f compose.base.yml -f compose.development.yml **

To debug the merges just use:

**docker-compose -f file1.yml -f file2.yml config **

**Read more:... [https://medium.com/@pscheit/docker-compose-advanced-configuration-541356d121de](https://medium.com/@pscheit/docker-compose-advanced-configuration-541356d121de)**


#### Run PHPUnit tests inside a docker container from PhpStorm



1.  configure the connection with docker engine. Go to Settings -> Build, Execution, Deployment -> Docker.
1.  Add new docker connection, The only value you should need to change is the** API URL**:
    1.  Linux: unix:///var/run/docker.sock
    1.  Windows and Mac: http://127.0.0.1:2376 (The URL of the docker machine.)
1.  Create the remote interpreter
    1.  Settings → Languages & Frameworks → PHP → Remote → Docker
    1.  Finally select the interpreter path. Usually, PHP-based docker images have the php interpreter in the PATH, so just setting php should be enough.
1.  Create the PHPUnit config
    1.  Settings → Languages & Frameworks → PHP → PHPUnit → Remote
    1.  PhpStorm mounts the project dir as a volume inside the container, in the /opt/project dir, and based on that path, selects the path to composer's autoloader script and the phpunit.xml script.

[https://blog.alejandrocelaya.com/2017/02/01/run-phpunit-tests-inside-docker-container-from-phpstorm/](https://blog.alejandrocelaya.com/2017/02/01/run-phpunit-tests-inside-docker-container-from-phpstorm/)

**Debug using docker + phpstrom**

[https://gist.github.com/chadrien/c90927ec2d160ffea9c4](https://gist.github.com/chadrien/c90927ec2d160ffea9c4)

[https://serversforhackers.com/c/getting-xdebug-working](https://serversforhackers.com/c/getting-xdebug-working)


### Dockerizing a Node.js web app

[https://nodejs.org/en/docs/guides/nodejs-docker-webapp/](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/)



1.  [Creating a nodejs app](https://drive.google.com/open?id=1mcda1Ydx_t7K1jWf_PRl4h3LWaS8Foigzd_0NlVjgjs)
1.  Creating a **Dockerfile**

touch Dockerfile


<table>
  <tr>
   <td><strong>FROM </strong>node:8
<p>
# Create app directory
<p>
<strong>WORKDIR /</strong>usr<strong>/</strong>src<strong>/</strong>app
<p>
# Install app dependencies
<p>
# A wildcard is used to ensure both package.json AND package-lock.json are copied
<p>
# where available (npm@5+)
<p>
<strong>COPY </strong>package<strong>*</strong>.json .<strong>/</strong>
<p>
<strong>RUN </strong>npm install
<p>
# If you are building your code for production
<p>
# RUN npm install --only=production
<p>
# Bundle app source
<p>
<strong>COPY </strong>. .
<p>
<strong>EXPOSE </strong>8080
<p>
<strong>CMD </strong>[ "npm", "start" ]
   </td>
   <td><strong>node_modules</strong>
<p>
<strong>npm-debug.log</strong>
   </td>
  </tr>
</table>


Why we are only copying the package.json file.?

This allows us to take advantage of cached Docker layers [more](http://bitjudo.com/blog/2014/03/13/building-efficient-dockerfiles-node-dot-js/)



1.  Create **.dockerignore** file
1.  Building your image \
following command to build the Docker image. The -t flag lets you tag your image so it's easier to find later using the docker images command:

<table>
  <tr>
   <td>
docker build <strong>-</strong>t <strong><</strong>your username<strong>>/</strong>node-web-app .
   </td>
   <td>docker images
   </td>
  </tr>
</table>




1.  Run the image

Running your image with -d runs the container in detached mode, leaving the container running in the background. The -p flag redirects a public port to a private port inside the container.


```
docker run -p 49160:8080 -d <your username>/node-web-app

# Get container ID
$ docker ps

# Print app output
$ docker logs <container id>

# Example
Running on http://localhost:8080

# Enter the container/ Access BASH on container
$ docker exec -it <container id> /bin/bash
```




1.  Test the image

<table>
  <tr>
   <td>
$ docker ps
<p>
# Example
<p>
ID            IMAGE                                COMMAND    ...   PORTS
<p>
ecce33b30ebf  <strong><</strong>your username<strong>>/</strong>node-web-app:latest  npm start  ...   49160-<strong>></strong>8080
   </td>
  </tr>
  <tr>
   <td>$ curl <strong>-</strong>i localhost:49160
<p>
HTTP<strong>/</strong>1.1 200 OK
<p>
X-Powered-By: Express
<p>
Content-Type: text<strong>/</strong>html; charset=utf-8
<p>
Content-Length: 12
<p>
ETag: W<strong>/</strong>"c-M6tWOb/Y57lesdjQuHeB1P/qTV0"
<p>
Date: Mon, 13 Nov 2017 20:53:59 GMT
<p>
Connection: keep-alive
<p>
Hello world
   </td>
  </tr>
</table>


In the example above, Docker mapped the 8080 port inside of the container to the port 49160 on your machine.


### Dockerizing a GraphQL app


### Dockerizing SCALA 


###### 
    **TABLE OF CONTENTS**



1.  USING DOCKER FOR SCALA
    1.  WHAT IS DOCKER?
    1.  WHAT IS SCALA?
1.  LET'S BEGIN
    1.  1. GATHERING RESOURCES
    1.  2. PREPARING THE DOCKERFILE
    1.  3. BUILDING THE IMAGE
    1.  4. PUSHING IMAGE TO DOCKER HUB
    1.  5. USING OUR CREATION


##### **USING DOCKER FOR SCALA**

In this guide, we will learn the process of setting up a simple Docker container for use with Scala. We will start by creating a simple Dockerfile to set up the environment and use it to build a Docker image. This image can be used to run a Docker container that allows us to compile Scala projects and even play around with the language in an interactive shell. All of this without muddying up our system or conflicting with other installations that we might have.


###### **WHAT IS DOCKER?**

While this guide's focus is not on the core concepts or use cases of Docker, it's good to have a basic idea about what it is before we dig in. Docker gives us a nice way to prepare an environment to handle a particular task without being invasive to our main operating system. It is important that we not confuse Docker and containers with a hypervisor and virtual machines, but the analogy may be useful in building an initial understanding. 

According to [docker.com](http://docker.com/):

Docker containers wrap a piece of software in a complete filesystem that contains everything needed to run: code, runtime, system tools, system libraries – anything that can be installed on a server. This guarantees that the software will always run the same, regardless of its environment.

This guide will display a casual convenience that Docker can provide to a developer, but its power goes far beyond this example. This guide only scratches the surface. For a much deeper explanation of Docker and its power, check out the [Docker Deep Dive](https://linuxacademy.com/cp/modules/view/id/33) course offered by Linux Academy.


###### **WHAT IS SCALA?**

Scala is a general-purpose programming language [with] full support for functional programming and a strong static type system. Designed to be concise, many of Scala's design decisions were inspired by criticism of Java's shortcomings.

via [Wikipedia](https://en.wikipedia.org/wiki/Scala_(programming_language))


##### **LET'S BEGIN**

Before we get started, you need to have [Docker installed](https://www.docker.com/products/docker) and have the daemon running (i.e. having the app running). Run the `docker -v` command from a Terminal window. If you see version information for Docker, then we are ready to go!

We will cover the following steps:



1.  Gather Resources: Scala requires Java, so we need to prepare our image with both.
1.  Prepare our Dockerfile: A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image.
1.  Build an image: An image is essentially a list of commands used to prepare the environment.
1.  Push the image to Docker Hub: The Docker Hub is an online repository for Docker images.
1.  Learn to use our Scala container: A container is an instance of an image. Our "Scala container" would then be an instance of the image we create in this guide.

If you care to push your image to the Docker Hub later on, go ahead and [sign up](https://hub.docker.com/) and remember your username.


###### **1. GATHERING RESOURCES**

Our goal is to create a Docker image that has been prepared to use Scala. We can then use this image to create containers (instances of an image) that let us use Scala. This allows us to avoid cluttering our operating system with installation files, environment variables, etc. Let's start out by preparing a directory for us to work in.

Open a terminal window and create/navigate to a fresh directory (referred to as 'working directory' from here on). For example:


```
mkdir ~/docker-scala
cd ~/docker-scala
```


Let's put the files we need (Scala and Java) in your working directory. You may do this any way you please, either by command line or GUI if you have it. We will be using an Ubuntu base image for our container, so choose accordingly:



*   [Java JDK Downloads](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
*   [Scala Downloads](http://www.scala-lang.org/download/)

Once you've downloaded both, verify that the two files are in your working directory. For the sake of this guide, we will assume they are named `jdk1.8.0_111.tar.gz` and `scala-2.12.0-M1.tgz`.


###### **2. PREPARING THE DOCKERFILE**

For our purposes here, you can think of a Dockerfile as a text file that lists how we want to prepare our image. You can read more about Dockerfiles, their keywords, and their formatting in the [Docker documentation](https://docs.docker.com/engine/reference/builder/#format) or in the [Docker Deep Dive](https://linuxacademy.com/cp/modules/view/id/33) course on Linux Academy.

Our Dockerfile will be very simplistic and only consist of a few sections:



*   Define the base image that we want to use. We will use Ubuntu in this example.
*   Define path variables for the Scala and Java installations.
*   Move the Scala and Java files over.
*   Provide a `CMD` to clean up usage

We need an empty file called `Dockerfile` in our working directory. You can create this from the command line using `touch Dockerfile` (be sure you have navigated to the folder first). Open this file up in your favorite text editor.

We use the `FROM` keyword to define the base image that we want to start with. Again, we will be using the `ubuntu` image, but you can use any image you like. If Docker doesn't see image locally, it will seek it out from Docker Hub. 

So let's define our base image in the `Dockerfile`:


```
# Define the environment
FROM ubuntu
```


You will often see images in the form "`image`:`tag`," but if you don't explicitly define a `tag`, then Docker assumes `latest`. Comments are denoted with a `#`.

Now we can set up the required PATH variables:

To ensure Scala and Java are in working order, we can tell the `Dockerfile` to set some environment variables for us. This is done with the `ENV` keyword:


```
ENV variableName value
```


Let's add this to our our Dockerfile:


```
# Define environment variables
ENV SHARE /usr/local/share
ENV SCALA_HOME $SHARE/scala
ENV JAVA_HOME $SHARE/java
ENV PATH=$SCALA_HOME/bin:$JAVA_HOME/bin:$PATH
```


It's time to move over those downloaded files:

The `ADD` keyword in a `Dockerfile` lets Docker know that we need to move something from our local filesystem to the image. It's important to notice that `ADD` will also unpackage these archives automatically, which is nice in our case!


```
ADD localDir remoteDir
```


Note: Remember that the files I downloaded to my working directory earlier were called `jdk1.8.0_111.tar.gz` and `scala-2.12.0-M1.tgz`. Yours are likely different, so just use the name of your files here.

Add this to the `Dockerfile`:


```
# Move over JDK and Scala
ADD jdk1.8.0_111.tar.gz /
ADD scala-2.12.0-M1.tgz /
```


This will unpackage and copy over the contents. At this point, the image would have two folders in the root directoy: `/jdk1.8.0_111` and `/scala-2.12.0-M1`. Now we want to use the `mv` command from Ubuntu to put them in the desired location. We can use the `RUN`keyword in the Dockerfile to indicate a command that we want to on the image as if it were running. 

Since we defined the `$SCALA_HOME` and `$JAVA_HOME` varilables earlier, we can use them here. Let's add this to our `Dockerfile`:


```
# Get JDK and Scala into place
RUN mv /jdk1.8.0_111 $JAVA_HOME
RUN mv /scala-2.12.0-M1 $SCALA_HOME
```


Lastly, we want to use the `CMD` keyword to help things act a little cleaner when we interact with our container later on. Add this final line to the `Dockerfile`:


```
CMD ["/bin/bash"]
```


At this point, our simple Dockerfile should be completed. Here's a summary of the entire file to make sure everything is correct.

Summary of `Dockerfile`'s contents:


```
# Define the environment
FROM ubuntu

# Define environment variables
ENV SHARE /usr/local/share
ENV SCALA_HOME $SHARE/scala
ENV JAVA_HOME $SHARE/java
ENV PATH=$SCALA_HOME/bin:$JAVA_HOME/bin:$PATH

# Move over JDK and Scala
ADD jdk1.8.0_111.tar.gz /
ADD scala-2.12.0-M1.tgz /

# Get JDK and Scala into place
RUN mv /jdk1.8.0_111 $JAVA_HOME
RUN mv /scala-2.12.0-M1 $SCALA_HOME

CMD ["/bin/bash"]
```



###### **3. BUILDING THE IMAGE**

In the previous step, we told Docker how we wanted our image to be set up. Now it's time to let it do its thing. We can use Docker's `build` functionality in terminal to prepare an image from this Dockerfile.


```
docker build <options> path
```


One `<option>` that we will use here is `-t` for tagging. My Docker Hub account name is `davisengeler` and I want to tag this image as `docker-scala`, so I want to use the tag `davisengeler/docker-scala`. Think of a proper tag for your image; we are about to create one.

In terminal, make sure you have navigated to the directory with our downloads and `Dockerfile` (example: `cd ~/docker-scala`). Now we can build the image with:


```
docker build -t davisengeler/docker-scala .
```


Note: Be sure to replace `davisengeler/docker-scala` with your own tag. The `.` at the end indicates the current directory in terminal. 

Docker now downloads any necessary resources from Docker Hub, starts a container from the base image, sets the variables for Scala and Java, extracts their contents, and puts them into place, then creates an image out of the container's final state. 

Troubleshooting: You may run into an issue with the `RUN mv ...` section. If the build fails because one of the folders doesn't exist, try this:



1.  Add `RUN ls` to your `Dockerfile` just above the `RUN mv...` commands. Save the file, return to terminal, and run the build command again. 
1.  You will still get the error, but you should see a list of files (bin, boot, dev, etc…). Within this list, there will be two listings that relate to Scala and Java. Take note of their exact names.
1.  Modify the two lines in your `Dockerfile` that start with `RUN mv ...` so the folder name matchs their respective listings you took note of in troubleshooting step above.
1.  Run the build command in Terminal again. If the build succeeded, I suggest removing the `RUN ls` line from your `Dockerfile` and calling the build command just one more time. 


###### **4. PUSHING IMAGE TO DOCKER HUB**

If all went as planned, you should see `Successfully built` and an ID. You can run `docker images` to make sure you see your new image in the list (you can identify it by the tag you created). If you care to publicly push this image up to your Docker Hub account, you can do so with Docker's `push` command in terminal. For example:


```
docker push davisengeler/docker-scala
```


Docker will push up the image as efficiently as possible. Anything used in an existing Docker Hub image will just be linked to your new one instead of re-uploading everything. Nice!

Now you should be able to see this image listed on your Docker Hub account page. 


###### **5. USING OUR CREATION**

So we've created an image for Scala and uploaded it to Docker Hub. Now it's time to get some use out of the thing! Remember the image name you used when creating it. Mine was `davisengeler/docker-scala`.

Interactive Scala Shell

We can use this container to play with Scala's interactive shell. It's a handy tool for trying out code or learning to use Scala. To get to it, we will use Docker's `run` command with the `-it`options to make it interative. Be sure to replace my image name with your own.


```
docker run -it davisengeler/docker-scala
```


This will start a container and put us in a bash prompt. Here we can run the `scala` to enter the interactive shell. Try it out with `print("Hey, Scala!")`. Use the `:quit` command to leave the Scala shell, then use "exit" to return to your main terminal prompt.

Compiling / Running a Scala Project

We can also use this container to compile and run Scala projects. For a very simple example, let's create a very simple Hello World in Scala. Create a local file `/scala_example/hello.scala` with the contents:


```
object Hello extends App {
  println("Hello, world")
}
```


We can compile and run this program with Scala on our container. Let's start a container with our local `/scala_example/` directory mounted as a volume (we can use the `-v SRC:DEST` command when we run the container):


```
docker run -it -v /scala_example:/scala_example davisengeler/docker-scala
```


Note: The `-it` option tells the container to run interactively. Don't forget to change my image name to your own.

We are presented with a bash prompt inside the container with the `/scala_example`directory mounted. Let's navigate to that directory, compile the program, and run it using `scalac` and `scala`:


```
cd /scala_example
scalac hello.scala
scala hello
```


Hello, world

You can exit the prompt with the `exit` command.


### Dockerizing the MEAN stack


#### INSTALL DOCKER

Docker actually has solutions for both [Mac](https://www.docker.com/products/docker#/mac) and [Windows](https://www.docker.com/products/docker#/windows), both of which allow us to install it using an executable. In this case, we are going to install Docker inside an Ubuntu instance, so we need to install it manually.

As they explain [on their documentation page](https://docs.docker.com/engine/installation/linux/ubuntulinux/), Docker requires a 64-bit Linux installation running version 3.10 or higher of the Linux kernel. To check both aspects, we can run uname command with -a flag:

**_uname -a_**

You will get something like the following:

Linux franverona2.mylabserver.com 3.13.0-106-generic #153-Ubuntu SMP Tue Dec 6 15:44:32 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux

The kernel version in this case is 3.13.0-106-generic, and x86_64 is telling us that we are running a 64-bit Linux distribution.

Now that we are sure that we can install Docker, we need to set apt to use packages from Docker repository:

sudo apt-get update \
sudo apt-get install apt-transport-https ca-certificates

After this, we have to add a new GPG key by downloading the key from a keyserver:

sudo apt-key adv \ \
  --keyserver hkp://ha.pool.sks-keyservers.net:80 \ \
  --recv-keys 58118E89F3A912897C070ADBF76221572C52609D

And finally, we will add the repository to our sources:

echo "deb https://apt.dockerproject.org/repo ubuntu-trusty main" | sudo tee /etc/apt/sources.list.d/docker.list

Now if we execute sudo apt-get update, our system will pull packages information from Docker repository too.

Docker also needs [aufs storage](https://en.wikipedia.org/wiki/Aufs) driver to work properly. For Ubuntu 14, this driver is present on linux-image-extra-* kernel packages. Using the uname command, we can extract our Linux version in order to install the right version:

sudo apt-get install linux-image-extra-$(uname -r) linux-image-extra-virtual

All is set, so we can now install Docker:

sudo apt-get update \
sudo apt-get install docker-engine

Docker works as a service, so we have to initialize its daemon:

sudo service docker start

Docker should now be installed and running. To check if everything is installed correctly, we can run a container using an example image called hello-world:

sudo docker run hello-world

This command will download a test image and run it in a container. This container simply prints a message and exit. If all works properly, you should see a message like this:

Hello from Docker! \
This message shows that your installation appears to be working correctly. \
To generate this message, Docker took the following steps: \
 1. The Docker client contacted the Docker daemon. \
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub. \
 3. The Docker daemon created a new container from that image which runs the \
    executable that produces the output you are currently reading. \
 4. The Docker daemon streamed that output to the Docker client, which sent it \
    to your terminal. \
To try something more ambitious, you can run an Ubuntu container with: \
 docker run -it ubuntu bash \
Share images, automate workflows, and more with a free Docker Hub account: \
 https://hub.docker.com \
For more examples and ideas, visit: \
 https://docs.docker.com/engine/userguide/

Docker binds to a Unix socket instead of a TCP port, so we need to be root in order to execute Docker commands. To avoid using sudo every time, we can add our user to the Docker group. In this case, we are going to add our current user, but we can specify a different user if necessary:

sudo usermod -aG docker $(whoami)

Now if we log out and log in into the system, our user will belong to the Docker group. We will be able to execute Docker commands without sudo.


#### SETTING UP DOCKER: DOCKERFILE

We can create custom images using a configuration file called Dockerfile. This file will include all required commands to create a Docker image with a custom configuration. Once the image is created, we can run as many containers as we need using it.

For our MEAN installation, we are going to create two different images: web server (Node.js and code) and database (MongoDB).


###### MONGODB IMAGE

The MongoDB image can be created using a Dockerfile, but we are going to use [the official one by MongoDB on DockerHub](https://hub.docker.com/_/mongo/). To use this image, we have to pull it from DockerHub repository using the pull command:

docker pull mongo

This will download the MongoDB image, but it won't start it. We will see how to run this image later, but we are keeping this on hold for now. Let's see how to create a web server image with Node.js.


###### NODE.JS IMAGE

To create our own web image, we are going to create a custom Dockerfile. This Dockerfile will be based on Ubuntu, and it will install all Node.js and all necessary dependencies.

Docker images (even those based on a distribution) will have the minimum packages required to run. In addition to the packages for the MEAN stack, we also need to install some others like gcc and make. It's also a great practice to clean temporary files and folders after installing packages to keep our Docker image as small as possible.

Let's start creating our Dockerfile. To create a file, we can use touch Dockerfile, then use your preferred text editor to start editing the file.

First of all, we have to specify our base image. In this case, our Docker image will be based on Ubuntu 14.04. We also specify our name as the maintainer of the image and a description (this has no effect on the execution, but it's usually a good idea to add relevant tags):

# We are going to use Ubuntu 14.04 \
FROM ubuntu:14.04 \
MAINTAINER Fran Verona \
LABEL Description="Dockerfile for MEAN stack"

We are going to expose some ports for our image. For MEAN stack, we need ports 3000 (Node.js) and 27017 (MongoDB), but you can expose as many ports as you need (we are also going to expose port 35729 for LiveReload because we are going to use it in our example later on). To expose these ports, we add the following to our Dockerfile:

# We need to expose ports for MongoDB (27017), Node.js (3000) and LiveReload (35729) \
EXPOSE 3000 27017 35729

We are now going to install a few essential packages and Git (prerequisite for MEAN stack). The essential packages are:



*   sudo: to allow privilege command execution.
*   
*   curl, gcc, make, build-essentials: for installing Node.js.
*   
*   git: Git control version.
*   

If we focus on *keeping Docker images as small as possible*, we should also remove cached packages, packages lists, and temporary files. We can do all of this with the following:

# Install prerequisites and essential packages \
RUN apt-get -q update && apt-get install -y -qq \ \
  git \ \
  curl \ \
  ssh \ \
  gcc \ \
  make \ \
  build-essential \ \
  sudo \ \
  apt-utils \ \
  && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

Now that we have all of the prerequisites and essential packages prepared, we are ready to install Node.js. There is a powerful script that will add the correct repository to our repositories list to ensure the correct installation.

We get this script using curl and execute it directly on bash. We will then install Node.js using apt, then remove cached packages, packages lists, and temporary files like before.

# Install Node.js \
RUN curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash - \ \
  && apt-get install -y -q nodejs \ \
  && apt-get clean \ \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

Node.js will also install npm as a package manager. We need it to install some MEAN dependencies, such as bower and gulp. As usual, we remove cached packages from npm:

# Install bower and gulp globally \
RUN npm install --quiet -g bower gulp \ \
  && npm cache clean

We will build our image using this Dockerfile:

docker build -t mean .

This will build a Docker image with Git and Node.js installed, but we can go even further and create an image with our MEAN project already deployed and ready to run. Let's take a look at an example!


#### EXAMPLE: MEAN.IO ON DOCKER

To illustrate how easy it can be to create a custom image for our MEAN project, we are going to modify our recent Dockerfile to create an image for [Mean.io project](http://mean.io/). This project is a MEAN boilerplate and is really easy to use. We are going to use our recent Dockerfile, so keep it open because the following will be added at the end of this file as a continuation.

In order to take a step back and view the big picture of what we need on our image now (remember that we have Node.js installed), we can enumerate our next steps:



*   Create the app directory.
*   
*   Copy app code into this directory. This copy can be a simple git clone from a repository.
*   Install server dependencies using npm and client dependencies using bower.
*   Start the server.
*   

So let's start by creating a directory for our app. Docker has an special command called WORKDIR to set a working directory in case we want to execute multiple commands in the same folder. This helps us a lot because we don't need to change directories manually. Instead, we set a working directory and all the following commands will work inside it.

To create a directory, we use mkdir Unix command with -p flag to make parent directories if needed, and set it as a working directory using WORKDIR from Docker:

RUN mkdir -p /usr/src/app \
WORKDIR /user/src/app

With a fresh working directory, we can copy the code of our app. We are going to clone the official repository:

# Clone Mean.io repository \
RUN git clone https://github.com/linnovate/mean.git /user/src/app

Now that we have our repo, we need to install server dependencies with npm and client dependencies with bower. For npm, we are using the --quiet flag for a less verbose output, and we also cleaned npm's cache:

# Install server dependencies using npm \
RUN npm install --quiet \ \
  && npm cache clean

For bower, we used the flag --config.interactive=false to set bower as a non-interactive. By default, bower may requires you to answer some questions regarding command itself (for example, it will ask if we want bower to collect usage statistics). By setting this parameter to false, we ensure that these questions will be answered automatically.

The bower install command will also give some ESUDO verbose output because it expects that we are privileged users. We can pass the --allow-root flag so bower will let us install packages:

# Install client dependencies using bower \
RUN bower install --config.interactive=false --quiet --allow-root

Now that we have all dependencies installed, we are ready to initiate our server. The latest version of Mean.io boilerplate has a Gulpfile which runs several commands to clean everything, compacts Javascript files, etc. To execute a command directly on a Docker container, we use the CMD command as follows:

CMD ["gulp"]

This is similar to execute the command gulp on a terminal.

Now we are ready to start our server, but let's review the contents of our Dockerfile:



*   Our Docker image uses Ubuntu 14.04
*   
*   We exposed some ports: MongoDB (27017), Node.js (3000) and LiveReload (35729).
*   
*   We did some installations, starting with essentials packages such as sudo or make, then Node.js, and finally bower and gulp.
*   
*   We set a working directory where we cloned our project repository.
*   
*   We installed server dependencies with npm and client dependencies with bower.
*   
*   Lastly, we start the server running a command (gulp in this case).
*   


#### RUNNING OUR CONTAINERS

As we said earlier, we are going to use two images: one for MongoDB (created by the official team and pulled from DockerHub), and one built by us from a custom Dockerfile.

To start a container using the MongoDB image, we use the run command with a few parameters:

docker run -p 27017:27017 -d --name db mongo

With this command, we are initiating a container using the MongoDB image (called mongo by default). We are using a few parameters for the run command:



*   MongoDB uses port 27017 by default, so we have to expose it on our container. To expose a port, we use the -p parameter.
*   
*   Unless instructed otherwise, Docker will attach the process of running a container to our current terminal. In this case, we don't need that, so we are going to run it as a background process using -d parameter. Feel free to not use it if you prefer to see the MongoDB output in real time.
*   
*   Docker will assign random names to our containers when we start them, but we can set our own name using --name parameter.
*   

In conclusion, this command will start a container called db using the image mongo, expose port 27017, and run it in the background with the -d parameter.

Now, if we do a docker ps command, we will see something like this:

CONTAINER ID     IMAGE     COMMAND                  CREATED           STATUS             PORTS                           NAMES \
825603e713ce     mongo     "/entrypoint.sh mo..."   1 minute ago      Up 1 minute        0.0.0.0:27017->27017/tcp        db

We now have a MongoDB container ready to be used. Let's create a new container for a web server with Mean.io using our Dockerfile.

For the MongoDB container, we didn't need to build anything because the image was hosted on Dockerhub, but in this case we have to build our custom image using build Docker command. If we are not in the folder where the Dockerfile is located, we should move to it (path/to/dockerfile is just an example, you should use your own):

cd path/to/dockerfile

Then we are ready to build the image. To keep things as organized as possible, we tag our image as mean:

docker build -t mean .

This process will take a while because our image will download an Ubuntu image (remember that our Docker image is based on Ubuntu 14.04), then install some packages and dependencies.

When the build is finished, we are ready to run the image:

docker run -p 3000:3000 -p 35729:35729 --name mean --link db:db mean

We are using a new parameter called --link where we specify that this container will be linked to the db container (our MongoDB container was named db using the --name parameter before).

Now if we open the browser and type our server IP plus Node.js default port (for example, 54.171.211.69:3000), we will see the Mean.io example project running. To get our server IP, we need to go to the LinuxAcademy site and click on Servers. We will see our server IP in the PUBLIC IP column for our server.


#### CONCLUSION

In this server lab, we learned how to use Docker to create containers for the MEAN stack. We also created our own image using the MEAN.io boilerplate as an example, and linked a container to a MongoDB image pulled from Dockerhub.

Docker has extensive documentation, and we can use advanced parameters such as [networking](https://docs.docker.com/engine/userguide/networking/) to create advanced infrastructures. Feel free to learn more about Docker and you will discover how powerful it can be.

 

[https://github.com/rlerdorf/php7dev](https://github.com/rlerdorf/php7dev)

 

[https://github.com/svpernova09/HomesteadSkeleton](https://github.com/svpernova09/HomesteadSkeleton)

 

[http://www.phpclasses.org/blog/post/295-How-to-Use-Vagrant-to-Improve-Your-Web-Development-Process.html](http://www.phpclasses.org/blog/post/295-How-to-Use-Vagrant-to-Improve-Your-Web-Development-Process.html)


Testing that your PHP code works on PHP7Dev

To test my PHP code, I share it into the VM. This is done in the Vagrantfile using the config.vm.synced_folderdirective.

I want to share my Zend Framework 1 source, so I edit Vagrantfile and after the config.vm.box line, I add this line:


    config.vm.synced_folder "/www/zendframework/zf1", "/www/zf1"

This maps my local source code which is at /www/zendframework/zf1 into the VM at the /www/zf1 directory.

Run vagrant reload after changing the Vagrantfile in order to effect the change.

I can now vagrant ssh into the VM, cd /www/zf1 and run my unit tests (after[ installing PHPUnit](http://akrabat.com/global-installation-of-php-tools-with-composer/), of course). If you want to run a website, then you need to set up a vhost as appropriate within the VM.

 

[http://akrabat.com/building-and-testing-php7/](http://akrabat.com/building-and-testing-php7/)

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

Homestead with PHP 7 Support

 

Laravel Homestead just received a new update with support for PHP 7 which is due out this month.

If you are already using the PHP 5.x Homestead box, you can upgrade your installation to PHP 7.0 by cloning the php-7 branch of the laravel/homestead repository into a new folder:

git clone -b php-7 https://github.com/laravel/homestead.git Homestead \
 \


Next add the box directive to the top of your existing Homestead.yaml file:

box: laravel/homestead-7 \
 \


Finally, you can run the vagrant up command from the directory that contains your clone of the laravel/homestead repository.

For more information consult the[ official documenation](http://laravel.com/docs/5.1/homestead#upgrading-to-php-7) and also checkout Laracasts[ PHP 7](https://laravel-news.com/2015/08/videos-to-learn-about-php-7/) video series.

 

 

[https://www.joeferguson.me/laravel-homestead-4-0-and-box-1-0/](https://www.joeferguson.me/laravel-homestead-4-0-and-box-1-0/)

 

 

Homestead

The host path of the shared folder is missing?



*   [https://www.vagrantup.com/docs/synced-folders/basic_usage.html](https://www.vagrantup.com/docs/synced-folders/basic_usage.html)

 

 

 

[https://gilbert.pellegrom.me/backing-up-laravel-homestead-databases/](https://gilbert.pellegrom.me/backing-up-laravel-homestead-databases/)

 

 

 

[https://mattstauffer.co/blog/introducing-laravel-homestead-2.0](https://mattstauffer.co/blog/introducing-laravel-homestead-2.0)

A Simple PHP Development Environment For OSX

[https://github.com/yuloh/camp](https://github.com/yuloh/camp)

[https://yuloh.github.io/2016/camp/](https://yuloh.github.io/2016/camp/)

[https://deployer.org/](https://deployer.org/)

[http://editorconfig.org/](http://editorconfig.org/)

[http://mamchenkov.net/wordpress/2016/09/16/bitbucket-pipelines-and-docker-for-php-developers/](http://mamchenkov.net/wordpress/2016/09/16/bitbucket-pipelines-and-docker-for-php-developers/)


[TOC]



# LINUX: {#linux}



1.  installation         -        sudo apt-get install php5-xdebug

            sudo service apache2 restart


			  



1.  disable xdebug   -       sudo php5dismod xdebug 
1.  enable xdebug   -       sudo php5enmod xdebug

4. look for xdebug options		php -i | grep xdebug


# MAC {#mac}



1.  sudo pecl install xdebug
1.  sudo nano -w /usr/local/etc/php/5.6/php.ini     check it with phpinfo()
1.  sudo /etc/init.d/apache2 restart             or 
1.  sudo apachectl restart

Can't have a PHP development environment without [xdebug](http://www.xdebug.org/)! Fortunately, Yosemite ships with it ready to enable:



1.  Edit /etc/php.ini and add this line to the end: \
zend_extension = "xdebug.so"
1.  If you want to configure your xdebug settings, then add an [xdebug] section. I like these settings (use with caution…): \
xdebug.var_display_max_children = 999 \
xdebug.var_display_max_data = 999 \
xdebug.var_display_max_depth = 100 \
xdebug.max_nesting_level = 200
1.  Restart apache: sudo apachectl restart and check in the phpinfo that xdebug is now loaded.


# Setup xdebug {#setup-xdebug}



1.  Go to the terminal, execute `php -i` and copy the output of this command.
1.  Go to [http://xdebug.org/wizard.php94](http://xdebug.org/wizard.php), past the output and press the button
1.  Follow the steps you see after pressing the button "Analyse my phpinfo() output"


# Xdebug with Homestead 4.0

[https://github.com/laravel/framework/issues/11594](https://github.com/laravel/framework/issues/11594)

[https://gilbert.pellegrom.me/setting-up-xdebug-on-homestead-4-php-7/](https://gilbert.pellegrom.me/setting-up-xdebug-on-homestead-4-php-7/)

[https://xdebug.org/files/](https://xdebug.org/files/)


## **Conditional Break points**

[https://gilbert.pellegrom.me/conditional-breakpoints-in-phpstorm/](https://gilbert.pellegrom.me/conditional-breakpoints-in-phpstorm/)

		

Here is a quick tip: Have you ever been debugging code in a loop and only wanted to trigger a breakpoint if a specific condition is met? In PhpStorm it's actually very simple. Right click on the breakpoint and add a "Condition".



<p id="gdcalert22" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/Environments20.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert23">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/Environments20.png "image_tooltip")


	

**How to Install MEAN on Ubuntu 16.04**


###### 
    **TABLE OF CONTENTS**



1.  INTRODUCTION
1.  PREREQUISITES
1.  INSTALL MONGODB
1.  INSTALL NODE.JS
1.  INSTALL PACKAGE MANAGERS: NPM AND BOWER
1.  CONCLUSION
1.  WHAT'S NEXT?


##### **INTRODUCTION**

**The MEAN stack is a collection of technologies used to develop web applications. This stack is formed by four technologies where it takes the acronym: MongoDB as a database system, Node.js as a server system, Express as a server side framework and Angular as a client side framework.**

**Unlike the classic LAMP stack (Linux, Apache, PHP, MySQL), we don't need to learn different programming languages to work both the client and server side. MEAN uses JavaScript for both.**

**This stack provides us mechanisms to create dynamic web applications. To achieve this, the MEAN stack uses a [NoSQL database system](https://en.wikipedia.org/wiki/NoSQL) where we define schemas for our models, but data can have variable sizes in the database. The stack also uses Angular which provides simple functions to handle data and synchronize data with client views using listeners.**

**In this guide, we will install all needed components, and create an example project using something called the Mean.io boilerplate.**


##### **PREREQUISITES**

**MEAN uses git as a version control for packages. You can check if you have it already installed executing:**


```
$ git
```


**If you receive a "command not found" error, you should install it using apt:**


```
$ sudo apt-get install git
```



##### **INSTALL MONGODB**

**Even though MongoDB is included in Ubuntu package repositories, they recommend us to use their repository in order to have the most updated version.**

**Ubuntu guarantees the authenticity of their packages by using the package's distributor public key, so we need to import the MongoDB public key first using the following command:**


```
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
```


**Then we add their repository to our Ubuntu repositories list:**


```
$ echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
```


**Now, we need to reload our local package database so Ubuntu can use our new repository when we look for the MongoDB package:**


```
$ sudo apt-get update
```


**And finally, we just install it by using the apt command:**


```
$ sudo apt-get install -y mongodb-org
```


**If everything goes well, MongoDB should be installed and automatically running as a service. To check if MongoDB is running correctly, we can execute the following command:**


```
$ service mongod status
```


**And it should return something like:**


```
mongod start/running, process 8247
```


**MongoDB has a configuration file located inside its installation folder (commonly /etc/mongod.conf) where we can define a lot of parameters like default port, the folder where the database will be stored, etc. If you need advanced features, you should check the [MongoDB](https://docs.mongodb.com/manual/reference/configuration-options/)<span style="text-decoration:underline;"> [File Options manual](https://docs.mongodb.com/manual/reference/configuration-options/)</span> for full specifications.**


##### **INSTALL NODE.JS**

**To install Node.js, we can download their code and compile it ourselves or install it directly from the repository. We are going to use the second option in this tutorial, but feel free to [download it](https://nodejs.org/en/download/) and compile it by yourself if you feel confident.**

**Node.js installation is quite similar to the MongoDB installation: we add a new repository and execute the apt command to install the package.**

**Node.js is more friendly because they provide us a script which will look for the best repository for our system. Using the following command will automatically read this script from a remote location and execute it:**


```
$ curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
```


**_NOTE: if you are curious, you can visit the url and take a look at the_**

**_script. You will learn a lot._**

**As we add the repository, we just install it using apt as usual:**


```
$ sudo apt-get install -y nodejs
```


**Node.js has no dependencies, but some npm packages will probably throw errors when compiling. To fix this, we need to install the build-essentials package as well:**


```
$ sudo apt-get install build-essential
```



##### **INSTALL PACKAGE MANAGERS: NPM AND BOWER**

**If you are familiar with web development, you are probably aware of frameworks and libraries like jQuery or Bootstrap which allow us to create powerful websites in an easy way.**

**When an application starts to grow and we add more and more libraries, the maintenance of these libraries (versions, upgrades, etc) became more difficult. To fix this, MEAN stack uses two powerful tools which simplify this task: _npm _and _bower_.**

**_npm _is a package manager for JavaScript commonly used in Node. You can find a lot of packages with powerful code ready to use, so you don't need to code it yourself. Some examples of npm packages are:**



*   **[s3](https://www.npmjs.com/package/s3): Amazon S3 high level client for handling S3 uploads (retries, secure requests, progress report, etc).**
*   **[sharp](https://github.com/lovell/sharp): high performance image processing to manipulate images (resize, crop, extract regions, etc).**
*   **[async](http://caolan.github.io/async/): powerful library with functions to allow us asynchronous operations (multiple functions in parallel, mapping, execute a function until something occurs, create queues, etc).**
*   **[mongoose](http://mongoosejs.com/index.html): schema validation, high level functions for saving and much more for MongoDB.**

**If you have already installed Node.js on your system, _npm _should be installed as well. You can see what version of _npm _are you using executing the following:**


```
$ npm -v
```


**If you need more help with _npm_, you can [check their page](https://www.npmjs.com/) for further information about parameters, packages, etc.**

**On the other side, we have _bower _as a package manager for the client side. Some examples of bower packages widely used by a lot of web apps are:**



*   **jQuery: most popular JavaScript library with powerful high level functions to manipulate DOM elements, make Ajax calls, etc.**
*   **Bootstrap: framework made by Twitter which provides elements like sliders, tooltips, etc.**
*   **[moment](https://github.com/moment/moment): to parse and manipulate dates.**
*   **[Font Awesome](https://github.com/FortAwesome/Font-Awesome): font made of icons.**

**_bower _needs to be installed manually but...surprise! We need npm first, so be sure that you have already installed it on your system. If you have npm, execute this command to begin the bower installation:**


```
$ sudo npm install -g bower
```


**The -g option is a parameter to install this [package as global](https://docs.npmjs.com/getting-started/installing-npm-packages-globally). This means that you will be able to execute _bower _command from everywhere in your system. If you don't want this behavior, just remove the -g option and _bower _will be installed in your current folder (we recommend to install _bower _as global).**

**Install gulp (optional)**

**There are several tasks which need to be done before a web app can go from development to production. More common tasks includes [minify](https://en.wikipedia.org/wiki/Minification_(programming)), [lint](http://stackoverflow.com/questions/8503559/what-is-linting), etc. To avoid wasting time on doing these tasks manually one by one, we can use _gulp_.**

**_gulp _is a task manager that helps us to define tasks and provide us a simple way to execute them in batch mode.**

**To install _gulp_, we use npm again as we did before with bower:**


```
$ sudo npm install -g gulp
```


**Our first MEAN application**

**Now that we have all technologies installed, let's code something! You can use your own folder structure and create everything on your own (your own Node.js server, your own Angular controllers, etc), or use a third-party boilerplate. For this guide, we are going to use the [Mean.io](http://mean.io/)<span style="text-decoration:underline;"> </span>framework.**

**_Mean.io _creates a sample project with a Node.js server already setup and a lot of useful packages, such as Passport (for user login and registration), Angular, Express for Node.js, jQuery, Bootstrap, Mongoose, etc.**

**To start creating our first project with _Mean.io_, we need to install their command line interpreter first:**


```
$ sudo npm install -g mean-cli
```


**Then we use the mean command to create a new _Mean.io_ project:**


```
$ mean init myapp
```


**Where "myapp" is the name of our application (a folder with this name will be created, so be sure that you're not using a duplicate name).**

**_Mean.io _interpreter will ask us to name our app, but we can press the Return key to use the same name as the folder name. When all is set, we need to go in our project folder:**


```
$ cd myapp
```


**And install all dependencies using npm (server side):**


```
$ sudo npm install
```


**And bower (client side):**


```
$ bower install
```


**This will take a couple of minutes because packages need to be downloaded and installed, so be patient.**

**When everything is installed, execute gulp to initialize the project:**


```
$ gulp
```


**_Mean.io_ has a powerful gulp file with a lot of tasks such as linter, minify (only in production environment), auto reload on changes, etc. If you want to go deeper and customize this, take a look at some [gulp recipes](https://github.com/gulpjs/gulp/tree/master/docs/recipes#recipes) to see how it works.**

**Finally, if you go to your browser and type [http://localhost:3000](http://localhost:3000/), you will see something like this:**



<p id="gdcalert23" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/Environments21.jpg). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert24">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/Environments21.jpg "image_tooltip")



##### **CONCLUSION**



1.  **We have learned what MEAN stack means, and which technologies it is made up of.**
1.  **We have installed MongoDB and Node.js in our system, along with both npm and bower as package managers.**
1.  **We learned what npm and bower are, and how powerful and useful they can be.**
1.  **We also created a sample project using the Mean.io framework.**


##### **WHAT'S NEXT?**

**This is an introduction guide on how to install all technologies which make up the MEAN stack, but we didn't cover other aspects more related to building appications. In future guides, we will learn how we can interact with our Mongo database, create our own Angular controller, and more.**


<!-- Docs to Markdown version 1.0β14 -->
