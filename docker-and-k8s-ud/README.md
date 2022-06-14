#### Copying Files Into & From Container

1. Copy To The Container: `docker cp  test.text or . (to coppy all files)  <ContainerName>:/<PathOnContainerToCopy>`
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


