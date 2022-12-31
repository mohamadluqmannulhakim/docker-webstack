FROM node:19-alpine3.16
RUN mkdir -p /var/www/app
COPY ./src/ /var/www/app
EXPOSE 3000

WORKDIR /var/www/app
RUN npm install -g nodemon && npm install
# COPY . /

# RUN mkdir -p /var/www/app
# WORKDIR /var/www/app
# COPY . /var/www/app
# RUN npm install -g gulp pm2 notify-send
# RUN npm install 