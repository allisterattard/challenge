FROM node:latest AS webapp
WORKDIR /app

COPY package.json package.json
RUN npm install

COPY public/ public
COPY src/ src
RUN npm run build

FROM nginx:alpine
COPY --from=webapp /app/build/ /usr/share/nginx/html
