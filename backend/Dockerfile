# Use an official Node runtime as a parent image
FROM node:14 AS build

WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .

# Add multi-stage build
FROM alpine:3.16.7

WORKDIR /app

# Install both nodejs and npm in the final image
RUN apk update && apk add --update nodejs npm

COPY --from=build /usr/src/app /app

EXPOSE 5000

CMD ["npm", "start"]
