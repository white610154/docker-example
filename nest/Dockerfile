FROM node:13.12.0

RUN mkdir -p /app
WORKDIR /app

COPY ./app /app

# Set the user to use when running this image
#USER node

RUN npm i -g @nestjs/cli

RUN npm install
