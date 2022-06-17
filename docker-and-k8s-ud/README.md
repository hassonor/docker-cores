#### Copying Files Into & From Container

1. Copy To The Container: `docker cp  test.text or . (to copy all files)  <ContainerName>:/<PathOnContainerToCopy>`
2. Copy From the Container: `docker cp <ContainerName>:/<PathOnContainer> <LocalPathToCopyFromContainer>`

#### Naming and Tagging Containers & Images

1. Create Container with Custom
   name: `docker run -p [extenalPort]:[internalPort] -d --rm --name <ContainerName> <ImageId>`
2. Create an image with name:tag: `docker build -t <ImageName>:<ImageTag> .`
3. Run the Container of the previous created
   image: `docker run -p [externalProt]:[internalPort] -d --rm --name <ContainerName> <ImageName>:<ImageTag>`
4. Remove All images includes tagged images: `docker image prune -a`

# Images & Containers

## Images

Images are **one of the two core building blocks** Docker is all about (the other one is
"Containers").<br>
Images are **blueprints / templates** for containers. They are **read-only** and contain the
application as well as the necessary application environment (operating system, runtimes, tools,
...).<br>
Images **do not run themselves** , instead, they can be executed as containers.
Images are **either pre-built** (e.g. official Images you find on DockerHub) or you build **your own**
Images by defining a `Dockerfile`.<br>
Dockerfiles contain **instructions** which are executed when an image is built (`docker build .`),
every instruction then creates a **layer** in the image. Layers are used to **efficiently rebuild** and
share images.<br>
The `CMD` instruction is special: It's **not executed when the image is built** but when a **container
is created and started** based on that image.

## Containers

Containers are the **other key building block** Docker is all about.<br>
Containers are **running instances of Images**. <br><br>
When you create a container (via `docker run`),
a thin **read-write layer** is added on top of the Image.<br><br>
**Multiple Containers can therefore be started based on one and the same Image**. <br><br>
All Containers run in **isolation** , i.e. they don't share any application state or written data.<br><br>
You need to create and start a Container to start the application which is inside a Container.<br><br>
So it's Containers which are in the end executed - both in **development and production**.

## Key Docker Commands

For a full list of all commands, add `--help` after a command - e.g. `docker --help`, `docker run
--help` etc.<br><br>
Also view the official docs for a full, detailed documentation of ALL commands and features:
[https://docs.docker.com/engine/reference/run/](https://docs.docker.com/engine/reference/run/)<br>

* `docker build .`: Build a Dockerfile and create your own Image based on the file

* `-t NAME:TAG`: Assign a `NAME` and a `TAG` to an image

* `docker run IMAGE_NAME`: Create and start a new container based on image `IMAGENAME` (or
  use the image id)
    * `--name NAME`: Assign a `NAME` to the container. The name can be used for stopping and
      removing etc.
    * `-d`: Run the container in **detached** mode - i.e. output printed by the container is not
      visible, the command prompt / terminal does NOT wait for the container to stop
    * `-it`: Run the container in " **interactive** " mode - the container / application is then
      prepared to receive input via the command prompt / terminal. You can stop the
      container with `CTRL + C` when using the `-it` flag.
    * `--rm`: Automatically **remove** the container when it's stopped


* `docker ps`: **List** all **running** containers
    * `-a`: **List** all **containers** - including **stopped** ones
* `docker images`: **List** all **locally stored images**
* `docker rm CONTAINER`: **Remove** a container with name `CONTAINER` (you can also use the
  container id)
* `docker rmi IMAGE`: **Remove** an image by name / id
* `docker container prune`: **Remove** all **stopped** containers
* `docker image prune`: **Remove** all **dangling** images (untagged images)
    * `-a`: **Remove** all **locally stored** images
* `docker push IMAGE`: **Push** an image **to DockerHub** (or another registry) - the image name/
  tag must include the repository name/ url
* `docker pull IMAGE`: **Pull** (download) an image **from DockerHub** (or another registry) - _this
  is done automatically if you just `docker run IMAGE` and the image wasn't pulled before_

### Volumes and Bind Mounts

1. `docker run -v /app/data...` -> Anonymous Volume
2. `docker run -v data:/app/data...` -> Named Volume
3. `docker run -v /path/to/code:/app/code...` -> Bind mount volume

### Env vars

1. Loading .env
   file: `docker run -p 3003:3003 --env-file ./.env -d --rm --name feedback-app -v feedback:/app/feedback -v /app/node_modules -v ${PWD}:/app feedback-node:volumes`
2. Build with dev Args example: `docker build -t feedback-node:dev --build-arg DEFAULT_PORT=8000 .`

## Data and Volumes

___
**images are read-only** once they are created, they can't change(we have to rebuild them to update them).
<br><br>
**Containers on the other hand can read and write** - they add a thin "**read-write layer**" on top of the image.
That means that can make changes to the files and folders in the image without actually changing the image.
<br><br>
But even with read-write Containers, **two big problems** occur in many applications using Docker:

1. **Data written in a Container doesn't persist:** If the Container is stopped and removed, all data written in the
   Container is lost.
2. **The Container can't interact with the host filesystem:** if we change something in your host project folder,
   those changes are not reflected in the running container. We need to rebuild the image (which copies the folders) and
   start a new container.
   <br><br>
   **Problem 1** can be solved with a Docker feature called **"Volumes**". **Problem 2** can be solved by using **"Bind
   Mounts"**.

### Volumes

___
Volumes are folder (and files) managed on our host machine which are connected to folders / files inside a container.
<br><br>
There are **two types of Volumes**:<br>

* **Anonymous Volumes**: Created via `-v /some/path/in/container` and **removed automatically** when a container is
  removed because of `--rm` added on the `docker run` command.
* **Named Volumes**: Created via `via some-name:/some/path/in/container` and **NOT removed** automatically.
  <br><br>

With Volumes, **data can be passed into a container** (if the folder on the host machine is not empty) and it can be
saved when written by a container(changes made by the container are reflected on our host machine).
<br><br>
**Volumes are created and managed by Docker** - as a developer, you don't necessarily know
where exactly the folders are stored on your host machine. Because the data stored in there is
**not meant to be viewed or edited by us** - use "Bind Mounts" if we need to do that!
<br><br>
Instead, especially **Named Volumes** can help you with **persisting data**.
<br><br>
Since data is not just written in the container but also on our host machine, the **data survives even if a container is
removed** (because the Named Volume isn't removed in that case).
Hence, we can use Named Volumes to persist container data(e.g. log files, uploaded files, database files etc.);
<br><br>
Anonymous Volumes can be useful for ensuring that some Container-internal folder is **not overwritten** by a "Bind
Mount" from example.
<br><br>
By Default, **Anonymous Volumes are removed** if the Container was started with the `--rm` option
and was stopped thereafter. There are **not removed** if a Container was started (and then removed)
without that option.
<br><br>
**Named Volumes are never removed**, we need to do that manually (via `docker volume rm VOL_NAME`,see reference below).
<br><br>

### Bind Mounts

___
Bind Mounts are very similar to Volumes - the key difference is, that you, the developer, **set the
path on your host machine** that should be connected to some path inside a Container.
<br><br>
You do that via `-v`
<br>`/absolute/path/on/our/host/machine:/some/path/inside/of/container`.<br><br>
The path in front of the `:` (i.e. the path on our host machine, to the folder that should be shared
with the container) has to be an absolute path when using `-v` on the `docker run` command.
<br><br>
Bind Mounts are very useful for **sharing data with a Container** which might change whilst the
container is running - e.g. our source code that we want to share with the Container running
our development environment.
<br><br>
**Don't use Bind Mounts if you just want to persist data** - Named Volumes should be used for
that (exception: We want to be able to inspect the data written during development).
<br><br>
In general, **Bind Mounts are a great tool during development** - they're not meant to be used in
production (since our container should run isolated from its host machine).
<br><br>

## Key Docker Commands

___

* `docekr run -v /path/in/container IMAGE`: Create an **Anonymous Volume** inside a Container.
* `docker run -v some-name:/path/in/container IMAGE`: Create a **Named Volume** (named `some-name`) inside a Container.
* `dokcer run -v /path/on/our/host/machine:path/in/container IMAGE`: Create a **Bind Mount** and connect
  a local path on your host machine to some path in the Container.
* `docker volume ls`: **List all currently active / stored Volumes** (By all Containers).
* `docker volume create VOL_NAME`: **Create a new (Named) Volume** named `VOL_NAME`. You typically don't
  need to do that, since Docker created them automatically for you if they don't exist when running a container.
* `docker volume rm VOL_NAME`: **Remove a Volume** by its' name (or ID).
* `docker volume prune`: **Remove all unused Volumes(i.e. not connected to a currently running or stopped container).

### Volume Usage Scenarios

* Store log files Outside of container
* Store database files outside of container
* Link source code to container
* And Many more Scenarios...

## Networks / Requests

___
In many application, we will need more than one container - for **two main reasons**:

1. It's considered a **good practice** to focus each container on **one main task**.
2. It's **very hard** to configure a Container that **does more than one "main thing"**.

Multi-Container apps are quite common, especially if we are working on "real application".
<br><br>
Ofter, some of these Containers need to **communicate** through:

* either **with each other**
* or with the **host machine**
* or with the **world wide web**

### Communicating with the World Wide Web (WWW)

___
Communicating with the WWW(i.e. sending HTTP request or other kinds of Requests to other servers) is thankfully very
easy.
<br><br>
Consider this JavaScript example - though it'll always work, no matter which technology we are
using: <br>
`fetch('https://some-api.com/my-data').then(...)`
<br><br>
This very basic code snippet tries to send a `GET` request to `some-api.com/my-data`;
<br><br>
This will **work out of the box**, no extra configuration is required. The application, running in a Container, will
have no problems sending this request.

### Communicating with the Host Machine

___
Communicating with the Host Machine (e.g. because we have a database running on the Host
Machine) is also quite simple, though it **doesn't work without any changes**.
<br><br>
**One important note:** *If we deploy a Container onto a server (i.e. another machine), it's very unlikely
that we will need to communicate with that machine. Communicating to the Host Machine typically is a
requirement during development - for example because we are running some development database on
our machine.* <br><br>
Again, consider this JS example: <br><br>
`fetch('localhost:3000/demo').then(...)`
<br>
This code snippet tries to send a `GET` request to some web server running on the local host machine(i.e. **outside**
the Container but **not** the WWW).
<br><br>
On our local machine, this would work - inside a Container, it **will fail**. Because `localhost` inside of the
Container refers to the Container environment, **not to our local host machine which is running the Container / Docker.
<br><br>
But Docker has got us covered.
<br><br>
We just need to change this snippet like this:<br>
`fetch('host.docker.internal:3000/demo').then(...)`
<br><br>
`host.docker.internal` is a special address / identifier which is translated to the IP address of the machine hosting
the Container by Docker.
<br><br>
**IMPORTANT**: "Translated" does **NOT** mean that Docker goes ahead and changes the source code. Instead,
it simply detects the outgoing request and is able to resolve the IP address for that request.

### Communicating with Other Containers

___
Communicating with other Containers is also quite straightforward. We have two main options:
<br>

1. **Manually find out the IP** of the other Container (it may change though).
2. Use **Docker Networks** and put the Communicating Containers into the same Network.
   <br><br>
   Option 1 is not great since you need to search for the IP on our own, and it might change over time.
   <br><br>
   Option 2 is prefect though. With Docker, we can create Networks via `docker network create NETWORK_NAME` and
   we can then attach multiple Containers to one and the same Network.<br><br>

For Example:<br>

```shell
docker run -network my-network --name container_1 some-image
docker run -network my-network --name container_2 some-other-image
```

Both `container_1` and `container_2` will be in the same Network.
<br><br>
Now, we can simply **use the Container names** to let them communicate with each other,
Docker will resolve the IP for us (see above).<br><br>
`fetch('container_1/my-data').then(...)`

## Docker Compose

___
Docker Compose is an **additional tool**, offered by the Docker ecosystem, which helps with
**orchestration / management** of multiple Containers. It can also be used for single Containers to simplify building
and launching.

### Docker Compose Files

___
A `docker-compose.yaml` file looks like this:

```yaml
version: "3.8" # version of the Docker Compose spec which is being used
services: # "Services" are in the end the Containers that our app needs
  web:
    build: # Define the path to out Dockerfile for the image of this container
      context: .
      dockerfile: Dockerfile-web
    volumes: # Define any required volumes / bind mounts
      - logs:/app/logs
  db:
    build:
      context: ./db
      dockerfile: Dockerfile-web
    volumes:
      - data:/data/db
```

We can find the full **list of configurations**
here: [https://docs.docker.com/compose/compose-file/](https://docs.docker.com/compose/compose-file/)
<br><br>
**Important to keep in mind**: When using Docker Compose, we **automatically get a Network for all our Containers** -
so we don't need to add our Network unless we need multiple Networks.

### Docker Compose Key Commands

___
There are two key commands:<br><br>

* `docker-compose up`: **Start all containers** / services mentioned in the Docker Compose file.
    * `-d`: Start in **detached mode**
    * `--builld`: Force Docker Compose to re-evaluate / rebuild all images (otherwise, it only does that if an image is
      missing);
* `docker-compose down`: **Stop and remove** all containers / services
    * `-v`: **Remove all Volumes** used for the Containers - otherwise they stay around, even if the Containers are
      removed.
* `docker-compose up -d --no-deps [services...]` -
    * `--no-deps` flag - Do not recreate services that the service depends on
    * `[service]` - Stop,destroy and recreate *only* a specific service.
* `docker-compose logs optional:[services...] ` - View the logs for all Docker Compose services or mentioned services
    * we can also add flag `--tail=[INT]` to limit the amount of logs that are shown.
    * we can also add flag `--follow` to follow the logs.

Of course, there are **more commands**.
Official command reference here: [https://docs/docker.com/compose/reference/](https://docs.docer.com/compose/reference/)

### Shell into a Container

___

#### Using docker exec to Shell into a Container

* `docker exec -it <contianerId> sh`
    * `-it` flag for Interactive TTY
    * `sh` Shell to use

#### Shell Scenarios

* View log files stored in the container
* Run a DB seeder script
* View code or files in a container

#### Shell Into a Container with Docker Compose

* `docker-compose exec <serviceName> <shell>`

### Scale Containers

___

#### Defining an Environment Variable

* `docker-compose up -d --scale api=4`
    * `--scale` - Scale the number of containers created for a service
    * `api=4` - Define service(s) to scale








