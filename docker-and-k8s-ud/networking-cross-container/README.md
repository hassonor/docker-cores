1. `docker build -t favorites-node .`
2. `docker network create favorites-net`
3. `docker run -d --name mongodb --network favorites-net mongo`
4. `docker run --name favorites --network favorites-net -d --rm -p 3000:3000 favorites-node`

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


