1. `docker build -t goals-react .`
2. `docker run -e CHOKIDAR_USEPOLLING=true -v ${PWD}/src:/app/src --name goals-frontend --rm -d -p 3000:3000 -it goals-react`
