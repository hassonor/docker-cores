FROM node:14

WORKDIR /app

COPY package.json package-lock.json ./

RUN npm install

COPY . .

ARG DEFAULT_PORT=3003

ENV PORT $DEFAULT_PORT

EXPOSE $PORT

CMD ["npm", "start"]

