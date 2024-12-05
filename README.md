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

#### Functionalities of NGINX as a Proxy Server 

## Load Balancing 

Extension for English language correction

When a large number of requests goes to NGINX servers NGINX can act as a Load Balancer

> **Distributes incoming traffic** across multiple backend servers 

> To balance the load, improve performance and provide redundancy 

**Proxy** = General term meaning action on behalf of another 

**Proxy Server** = Intermediary server that forwards client requests to other servers 

#### Some Load Balancing Methods include 

**Least Connections**
```
Routes traffic to the server with the fewest active connections  
```
**Round Robin**
```
Distributes clients requests in a **sequential, cyclical manner to each server in the group
```  

## Caching

#### Cashing is a core feature of NGINX Proxy  

Instead we want to keep one file copy

> Do the heavy lifting once 

> And store the response 

> Cache responses from backend server for frequently accessed resources 

> Copies are stored temporarily to **improve the performance**

And send it to everyone who requested 

## Security

Only one server that is publicly available 

> Consolidated Security 

> Centralized Access Control 

> Minimized Exposure 

> Centralized Logging and Monitoring 

Only one Entrypoint 

#### Encrypted Communication 

> SSL/Termination Offloading 

> NGINX can handle SSL/TLS encryption and decryption 

NGINX forwards the encrypted message 

> Web server decrypts the message itself 

SSL ( Enforce HTTPS )

> **Accept** encrypted traffic 

> **Deny** non encrypted requests 

## Compression 

NGINX Proxy can compress the response ( Ex: Video of Netflix )

> To reduce bandwidth usage and improve load times

## Segmentation - Sending response in chunks 

> Breaks the file into smaller chunks ( Video Streaming )

## NGINX Configuration 

> The main config file is typically named **"nginx.conf"** and is usually located in the **"/etc/nginx/"** folder 

> Using a custom syntax comprising: **Directives** **Blocks** 

> This set up the **server's behavior** 

Web Server
```
Serving files itself
```
Proxy Server
```
Forward traffic to other web servers 
```


