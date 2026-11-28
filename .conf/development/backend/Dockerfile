FROM node:22-alpine

# Installing libvips-dev for sharp Compatibility
RUN apk update && apk add --no-cache build-base gcc autoconf automake zlib-dev libpng-dev nasm bash vips-dev git

WORKDIR /opt/
COPY ./backend/package.json ./
COPY ./backend/package-lock.json ./
RUN npm install
ENV PATH /opt/node_modules/.bin:$PATH

WORKDIR /opt/app
COPY ./backend ./
RUN chown -R node:node /opt/app
USER node
RUN ["npm", "run", "build"]

CMD ["npm", "run", "develop"]
