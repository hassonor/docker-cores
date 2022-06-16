### Simple and Basic (Just for demo purpose!) hosting with Docker on AWS EC2 Linux Instance:

#### #1 Setup and connect EC2 Linux Instance:

1. Create EC2 Linux Instance.
2. Download PEM key pair to your local machine.
3. Connect the EC2 Linux Instance on AWS from your local machine.
4. On SSH EC2 terminal (after connecting):`chmod 400 <KeyPairPemFile>`
5. On SSH EC2 terminal: `ssh -i "<KeyPairFile>" ec2-user@<YourPublicDnsOfEc2Instance>`
6. On SSH EC2 terminal: Type `yes` and press `Enter Key`

#### #2 Install Docker on our VM:

1. `sudo yum upadte -y`
2. `sudo amazon-linux-extras install docker -y`
3. `sudo service docker start`

* In case we need non-aws installation for
  docker: [https://docs.docker.com/engine/install/ (under "Server").](https://docs.docker.com/engine/install/)

#### #3 Pushing our local Image to the cloud:

1. Login DockerHub
2. Create a Public Image Repository with the name `node-example-1` for example.
3. On Our Local Machine on the root project path: `docker build . -t node-dep-example-1`
4. `docker tag node-dep-example-1 orh87/node-example-1`
5. `docker login`
6. `docker push orh87/node-example-1`

#### #4 Running and Publishing the App on AWS (EC2):

1. On SSH EC2 terminal (after connecting): `sudo docker run -d --rm -p 80:80 orh87/node-example-1`
2. Go to Security Group Section on AWS.
3. Click on the Security group ID related to our EC2.
4. Press on **Inbound rules**.
5. Press on `Edit inbound Rules`.
6. Add Rule `HTTP` set Source `Anywhere` and save.
7. Copy the IPv4 Public of the EC2.
8. Open Your Browser on local machine: Paste the previous IPv4.
9. We will see our site :).

#### #5 Update the Image

1. Change some code from our src. e.g. edit the `<h1>` of our `welcome.html` file.
2. On Our Local Machine on the root project path: `docker build . -t node-dep-example-1`.
3. `docker tag node-dep-example-1 orh87/node-example-1`
4. `docker push orh87/node-example-1`
5. On SSH EC2 terminal (after connection): `sudo docker ps`
6. On SSH EC2 terminal: `sudo docker stop <ContainerName>`
7. On SSH EC2 terminal: `sudo docker pull`
8. On SSH EC2 terminal: `sudo docker run -d --rm -p 80:80 orh87/node-example-1`