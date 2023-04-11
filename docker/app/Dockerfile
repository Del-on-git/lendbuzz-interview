FROM node:16

WORKDIR /usr/src/app

COPY app.js ./
COPY package*.json ./

RUN npm install

EXPOSE 3000

CMD ["node", "app.js"]
