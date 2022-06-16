FROM        node:14

LABEL       author="Or Hasson"

WORKDIR     /app

COPY        package.json package-lock.json ./

RUN         npm install

COPY        . ./

EXPOSE 3000

CMD ["npm","start"]