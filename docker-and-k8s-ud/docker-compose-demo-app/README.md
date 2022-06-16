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

Of course, there are **more commands**.
Official command reference here: [https://docs/docker.com/compose/reference/](https://docs.docer.com/compose/reference/)
