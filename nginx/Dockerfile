FROM nginx:latest

RUN apt-get update && apt-get install -y vim && apt-get clean
COPY ./default.conf /etc/nginx/conf.d/default.conf
