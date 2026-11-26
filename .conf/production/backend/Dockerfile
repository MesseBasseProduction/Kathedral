# Creating multi-stage build for production
FROM node:20-alpine as build

ARG ARG_PORT
ARG ARG_BACKEND_URL
ENV BACKEND_PORT=$ARG_PORT
ENV BACKEND_URL=$ARG_BACKEND_URL
ENV NODE_ENV=production

RUN apk update && apk add --no-cache build-base gcc autoconf automake zlib-dev libpng-dev vips-dev git > /dev/null 2>&1

WORKDIR /opt/
COPY ./backend/package.json ./backend/package-lock.json ./
RUN npm install
ENV PATH=/opt/node_modules/.bin:$PATH
WORKDIR /opt/app
COPY ./backend .
RUN npm run build

# Creating final production image
FROM node:20-alpine
ENV NODE_ENV=production
ARG ARG_PORT
ARG ARG_BACKEND_URL
ENV BACKEND_PORT=$ARG_PORT
ENV BACKEND_URL=$ARG_BACKEND_URL

RUN apk add --no-cache vips-dev

WORKDIR /opt/
COPY --from=build /opt/node_modules ./node_modules
WORKDIR /opt/app
COPY --from=build /opt/app ./
ENV PATH=/opt/node_modules/.bin:$PATH

RUN chown -R node:node /opt/app
USER node
CMD ["npm", "run", "start"]
