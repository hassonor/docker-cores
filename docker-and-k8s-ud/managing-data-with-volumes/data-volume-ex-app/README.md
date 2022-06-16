1. `docker build -t feedback-node:volumes .`
2. `docker run -p 3003:3003 -d --rm --name feedback-app -v feedback:/app/feedback feedback-node:volumes`
3. Browse to: `localhost:3003`
4. Add Title:`Awesome` and Document Text and Press "Save";
5. visit `localhost:3003/feedback/awesome.txt`
6. `docker stop feedback-app`
7. `docker volume ls` -> We still has the `feedback` volume.
8. `docker run -p 3003:3003 -d --rm --name feedback-app -v feedback:/app/feedback -v /app/node_modules -v ${PWD}:/app feedback-node:volumes`
9. visit `localhost:3003/feedback/awesome.txt` -> We still have the data.

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

