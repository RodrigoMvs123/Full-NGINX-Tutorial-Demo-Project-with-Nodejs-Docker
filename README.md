### This is a project accompanying Nginx Crash Course

### Commands used in the tutorial

##### start nginx
`nginx`

##### get options
`nginx -h`

##### restart nginx
`nginx -s reload`

##### stop nginx
`nginx -s stop`  

##### print logs
`tail -f /usr/local/var/log/nginx/access.log`

##### restart nginx
`nginx -s reload`

##### create folder for nginx certificates
`mkdir ~/nginx-certs`
`cd ~/nginx-certs`

##### create self-signed certificate
`openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout nginx-selfsigned.key -out nginx-selfsigned.crt`


## Full NGINX Tutorial Demo Project with Nodejs Docker

- https://www.youtube.com/watch?v=q8OleYuqntY
- https://github.com/RodrigoMvs123/Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker.git

#### What is Nginx ?
#### What is it used for ?

1. Dockerize Node.js app
2. Run 3 web servers
3. Install and Configure NGINX as load balancer
4. Configure Secure HTTPS 

#### NGINX as Web Server

> A web server refers to both the physical machine and software running on the machine 

> Primary function is to **serve web pages** to client browser's 

NGINX is a high performance web server 

> Piece of software on a server, that handles HTTP requests