FROM        node:14

LABEL       author="Or Hasson"

WORKDIR     /app

COPY        package.json package-lock.json ./

RUN         npm install

COPY        . ./
EXPOSE      80

ENV MONGODB_USERNAME root
ENV MONGODB_PASSWORD secret


ENTRYPOINT  ["npm", "start"]