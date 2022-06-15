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