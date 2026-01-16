FROM node:25-alpine

ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=password

WORKDIR /home/app

COPY ./app .

CMD ["node", "server.js"]