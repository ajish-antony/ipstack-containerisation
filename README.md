# IP Location Finding of Website Visitors
## (ipstack+Docker Containerisation+NginxLB)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Here is a project in which makes use of the ipstack feature to collect the geolocation of IP addresses accessing the website. The scenario for the project is that there if there is high access to a website application and needs to track the IP address details directly from the Ipstack, it will be difficult. As there will be rate-limiting for the API calls if it is fetching directly from the ipstack. So in order to avoid the issue, elastic cache Redis in AWS has been utilized (Redis containers can also be used as per the requirements). So once the details have been fetched, further it will be delivered from the cache server. As mentioned it's applicable for certain scenarios only. 

The basic outline of the project is that IP stack has been made utilized for tracking the access details of the website with the API token. At the same time for caching these details, the elastic cache Redis feature has been utilized. Moreverove infrastructure has been deployed in the docker containerized. For load-balancing these containers, Nginx has been utilized in the same manner as the containers. 

`IP Stack` : IP stack is it  offers a powerful, real-time IP to geolocation API capable of looking up accurate location data and assessing security threats originating from risky IP addresses. Using the ipstack API we will be able to locate website visitors at first glance and adjust the experience and application accordingly.

## Features

- Sample IP-Location finding website
- Docker-compose have been used and easy to spin up the multiple containers
- Containers are load balanced using the Nginx

## Architecture

![
alt_txt
](https://i.ibb.co/VvJ95mH/ipstack.jpg)

## Pre-Requests

- Knowledge in Docker and docker compose
- Docker compose installed and configured
- Basic knowledge in ipstack and it's working
- Need an IP stack Login and API for location finding

## Behind the Code

> Consider the provided below as a sample compose file and if required update the values according to the requirement 

```sh
version: "3.9"

services:
  nginx:
    image: nginx:alpine
    container_name: nginx
    restart: always
    ports:
      - "80:80"
    networks:
      - ipstack
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf

  ipstack1:
    image: ajishantony2020/ipstack1:latest
    container_name: ipstack1
    restart: always
    networks:
      - ipstack
    environment:
      CACHING_SERVER: "${CACHING_SERVER}"
      IPSTACK_KEY: "${IPSTACK_KEY}"

  ipstack2:
    image: ajishantony2020/ipstack1:latest
    container_name: ipstack2
    restart: always
    networks:
      - ipstack
    environment:
      CACHING_SERVER: "${CACHING_SERVER}"
      IPSTACK_KEY: "${IPSTACK_KEY}"

  ipstack3:
    image: ajishantony2020/ipstack1:latest
    container_name: ipstack3
    restart: always
    networks:
      - ipstack
    environment:
      CACHING_SERVER: "${CACHING_SERVER}"
      IPSTACK_KEY: "${IPSTACK_KEY}"
networks:
  ipstack:
```
- nginx.conf

```sh
events {
    worker_connections   1000;
}

http {
   upstream backend {
     server ipstack1;
     server ipstack2;
     server ipstack3;
   }

   server {

      listen 80;
      location / {
          proxy_pass http://backend;
      }
   }
}
```

## User Instructions
### Docker Compose Installation
For detailed explanation of docker-compose Installation -  [docker-compose Installation](https://docs.docker.com/compose/install/)

```sh
 sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
 sudo chmod +x /usr/local/bin/docker-compose
 sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
 docker-compose --version
```
### IP Stack API 
- For getting the IP stack API, please go through the instructions provided on the website - https://ipstack.com/
- For getting the Free API KEY, click on the option "GET FREE API KEY" and follow the instructions

![
alt_txt
](https://i.ibb.co/w6LYKCQ/stack.jpg)

### Elastic Cache Redis Configuration

- For the configuration part of the elastic cache of REDIS, follow the instructions provided in the document and can be updated as per the requirements. [AWS ElastiCache for Redis](https://aws.amazon.com/getting-started/hands-on/setting-up-a-redis-cluster-with-amazon-elasticache/)

![
alt_txt
](https://i.ibb.co/0mY2zNJ/redis.jpg)
- At the same time the Redis container can be also made use of in the place of AWS elastic cache for Redis. For the same update, the docker-compose file with the Redis container details as below.

```sh
  redis:
    image: redis:latest
    container_name: redis
    restart: always
    networks:
      - ipstack
```

- Further moves forwards with cloning the repository. 
- Updates the values in the env file as per the requirements. 
- After the same, implement the same using docker-compose

```sh
docker-compose --env-file env.dev up -d
```
### Output

- Update with the IP of the which needs to find the geolocation as provided below. As per the sample page created, here it will display the details as provided below. 

![
alt_txt
](https://i.ibb.co/2Fv5Kpf/output.jpg)

## Conclusion 

Here I have made utilized use of ipstack and its features along with caching && containerization in docker.


### ⚙️ Connect with Me

<p align="center">
<a href="mailto:ajishantony95@gmail.com"><img src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white"/></a>
<a href="https://www.linkedin.com/in/ajish-antony/"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/></a>
