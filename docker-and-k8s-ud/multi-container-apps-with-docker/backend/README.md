1. `docker network create goals-net`
2. `docker run --name mongodb -v data:/data/db --rm -d --network goals-net -e MONGO_INITDB_ROOT_USERNAME=orh -e MONGO_INITDB_ROOT_PASSWORD=secret mongo`
3. `docker build -t goals-node .`
4. `docker run --name goals-backend -v ${PWD}:/app -v logs:/app/logs -v /app/node_modules -e MONGODB_USERNAME=orh --rm --network goals-net -p 80:80 -it goals-node`
