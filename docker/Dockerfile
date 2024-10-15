FROM node:14
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY ./src .
EXPOSE 3000
CMD ["npm", "start"]