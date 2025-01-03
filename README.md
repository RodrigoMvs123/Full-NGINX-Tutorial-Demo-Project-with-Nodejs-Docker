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

#### Serving file itself

> Example of web server serving static files 

> **"location"** directive defines how to server should process specific types of requests 

**Redirect** all HTTP requests to HTTPS

nginx.config 
```nginx 
server { 
    listen 80;
    server name example.com www.example.com;

    location / {
        root /var/www/example.com;
        index   index.html index.html;
    }
}
```

#### SSL/TLS Encryption Configuration 

Server content over **HTTPS** with SSL/TLS configured

nginx.config 
```nginx 
server { 
    listen 80;
    server name example.com www.example.com;

    location / {
        root /var/www/example.com;
        index   index.html index.html;
    }


server {
    listen 443 ssl;
    server name example.com www.example.com;

    # SSl Configuration 
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    # Security Headers
    add_header Strict-Transport_Security "max-age=31536000:includeSubDomains" always;
    #...

    location / {
        root /var/www/example.com;
        index index.html index.html;
    }

    }
}
```

#### Configure Loading Balancing 

nginx.conf
```nginx
http {
    upstream myapp1 {
        server srv1.example.com;
        server srv2.example.com;
        server srv3.example.com;
    }

    server {
        listen 80;

        location / {
            proxy pass http://myapp1;
        }
    }
}
```

> In the example, there are 3 instances of the app running on svr1-svr3

> All the requests are proxied to the server group myapp1

> Default to **round-robin** algorithm 

Selecting the least busy server 

nginx.conf
```nginx 
http {
    upstream myapp1 {
        least_conn;
        server srv1.example.com;
        server srv2.example.com;
        server srv3.example.com;
    }
}
```

#### Enable the Caching of Responses 

- https://nginx.org/en/docs/dirindex.html 

nginx.conf
```nginx
http {
    #...   
    proxy cache path /data/nginx/cache keys zone=mycache:10m; 
}
```

> Then include the **proxy_cache** directive in the context (protocol type, virtual server, or location) for which you want to cache server responses, specifying the zone name defined by the **keys_zone** parameter to the **proxy_cache_path** directive ( in this case, **mycache** ):

nginx.conf
```nginx
http {
    #...
    proxy cache path /data/nginx/cache keys zone=mycache:10m; 
    server {
        proxy_cache mycache;
        location / {
            proxy_pass https://localhost:8000;
        }
    }
}
```

## NGINX in Kubernetes 

#### NGINX as a Kubernetes Ingress Controller 

What is **Ingress Controller** ?

> A specialized load balancer for **managing ingress ( incoming ) traffic** in Kubernetes 

> It handles the **routing to the appropriated services** based on the rules defined in an ingress resource 

> **NOT publicly accessible** 

#### Cloud Platform Load Balancer 

> Cloud load balancer handles the incoming traffic from the internet 

> Forward request to the Ingress Controller inside the cluster 

> Cluster component is **never directly exposed** to publicly access 

> Intelligent routing based on path and host matching 

The AWS service for load balancing is called Elastic Load Balancing (ELB). It automatically distributes incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, IP addresses, Lambda functions, and virtual appliances.

> WWW / AWS Load Balancer / Ingress Controller routes Requests to Online-Cart or Request to Payment, etc... / Kubernetes Cluster ( Online-Cart, Payment, etc... ) 


## NGINX versus Apache 

Both widely used with **same functionality** 

#### Apache 

> Apache extended the functionality as a proxy 

> Highly customizable and extensible 

> Good choice for dynamic content handling and legacy support 

#### NGINX 

> Faster and more lightweight 

> Best suited for high performance environments and serving static content 

> Simple configuration 

> More popular in container world

## Web App Application 

#### Source Code
```
Visual Studio Code
Explorer 
Open Editors
index.html
```

index.html 
```html 
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Beautiful Landing Page</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            margin: 0;
            padding: 0;
            background-color: #f0f0f0;
            color: #333;
        }
        header {
            background: url('images/career-quiz.png') no-repeat center center/cover;
            color: #fff;
            padding: 100px 0;
            height: 150px;
            text-align: center;
        }
        header h1 {
            font-size: 3em;
            margin: 0;
        }
        header p {
            font-size: 1.2em;
        }
        nav {
            background: #333;
            color: #fff;
            display: flex;
            justify-content: space-around;
            padding: 15px 0;
        }
        nav a {
            color: #fff;
            text-decoration: none;
            font-weight: bold;
        }
        .container {
            padding: 20px;
        }
        .grid {
            display: flex;
            gap: 20px;
            flex-wrap: wrap;
        }
        .card {
            background: #fff;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            flex: 1;
            min-width: 280px;
            max-width: 300px;
            padding: 15px;
        }
        .card img {
            border-radius: 8px;
            width: 100%;
            height: auto;
        }
        footer {
            background: #333;
            color: #fff;
            text-align: center;
            padding: 20px 0;
            margin-top: 20px;
        }
    </style>
</head>

<body>
    <header>
    </header>
    <nav>
        <a href="#home">Home</a>
        <a href="#about">Best Courses</a>
        <a href="#services">Fun Tutorials</a>
        <a href="#contact">About TechWorld with Nana</a>
    </nav>
    <div class="container">
        <h2>TechWorld with Nana Programs</h2>
        <div class="grid">
            <div class="card">
                <img src="images/devops.png?crop=entropy&fit=crop&w=400&h=200" alt="Service 1">
                <h3>DevOps Bootcamp</h3>
                <p>Finally learn with structured guided course, all DevOps tools together</p>
            </div>
            <div class="card">
                <img src="images/it-beginners.png?crop=entropy&fit=crop&w=400&h=200" alt="Service 2">
                <h3>Software Development LifeCycle Course</h3>
                <p>Learn the entire software Development lifecycle, from developing, to testing, to provisioning server and deploying</p>
            </div>
            <div class="card">
                <img src="images/devsecops.png?crop=entropy&fit=crop&w=400&h=200" alt="Service 3">
                <h3>DevSecOps Bootcamp</h3>
                <p>If you wanna become a DevOps engineer on steroids, you can face this advanced bootcamp</p>
            </div>
        </div>
    </div>
    <footer>
        <p>&copy; TechWorld with Nana. All Rights Reserved.</p>
        <p>Follow us on:
            <a href="#" style="color: #3b5998;">Linkedin</a> |
            <a href="#" style="color: #00aced;">Twitter</a> |
            <a href="#" style="color: #e4405f;">Instagram</a>
        </p>
    </footer>
</body>

</html>
```

Open the web application in the Browser 

> Absolute Path

```
C:\Users\usuario\Desktop\Rodrigo\Full NGINX Tutorial Demo Project with Nodejs Docker\Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker\index.html
```

```
TWN Career Quiz 
Your Custom Recommended Path 

Home           Best Courses            Fun Tutorials               About Tech World with Nana

TechWorld with Nana Programs 

    DEVOPS                           ...                    ...
   BOOTCAMP
    by TWN

Devops Bootcamp 

Finally learn with structured 
guided course, all Devops tools 
together

                            TechWorld with Nana. All Rights Reserved.
                            Follow us on: Linkedin | Twitter | Instagram 
``` 

## To add Backend Server 

#### Source Code

```
Visual Studio Code
Explorer
Open Editors 
index.html
server.js
```

server.js
```javascript
const express = require('express');
const path = require('path');
const app = express();
const port = 3000;

const appName = process.env.APP_NAME

app.use('/images', express.static(path.join(__dirname, 'images')));

app.use('/', (req, res) => {
    res.sendFile(path.join(__dirname, 'index.html'));
    console.log(`Request served by ${appName}`);
});

app.listen(port, () => {
    console.log(`${appName} is listening on port ${port}`);
});
```

What is **package.json** ?

> Configuration file used in Node.js projects to manage the **project's dependencies** 

> Dependencies: Lists the packages ( and their version ) that the project depends on 

Download Node.js 

- https://nodejs.org/en/download/package-manager ( npm comes within Node.js )

#### Source Code
 
```
Visual Studio Code
Explorer
Open Editors 
index.html
server.js
package.json
```

package.json
```json 
{
    "name": "nginx-crash-course",
    "version": "1.0.0",
    "description": "A Node.js application serving a static HTML file, used for load balancing with NGINX.",
    "main": "server.js",
    "scripts": {
      "start": "node server.js"
    },
    "author": "Your Name",
    "license": "MIT",
    "dependencies": {
      "express": "^4.17.1",
      "path": "^0.12.7"
    }
  }
```

Installing **express** and **path** packages 

#### Visual Studio Code
Terminal 
```
npm install
``` 

To start web application **server.js**

#### Visual Studio Code
Terminal
```
node server.js 
> node app is listening on port 3000
```

**Localhost**

> Refers to the **local machine** that you are currently using 

> Developers use localhost to **simulate network requests to services running on their own machine** 

Open in the Browser 

- localhost:3000 ( Web Application served by Node.js Backend )

```
TWN Career Quiz 
Your Custom Recommended Path 

Home           Best Courses            Fun Tutorials               About Tech World with Nana

TechWorld with Nana Programs 

    DEVOPS                           ...                    ...
   BOOTCAMP
    by TWN

Devops Bootcamp 

Finally learn with structured 
guided course, all Devops tools 
together

                            TechWorld with Nana. All Rights Reserved.
                            Follow us on: Linkedin | Twitter | Instagram 
``` 

The act of **refresh** the page makes 

#### Visual Studio Code
Terminal
```
node server.js 
> node app is listening on port 3000
> Request served by node app
> Request served by node app
```

Instead of the browser request going directly into the express server on Node.js 

> It will first hit the NGINX as Proxy ( Server ) then NGINX will forward the request to Node.js

## Dockerize the Web Application

> We will start **3 instances** of the Node.js server ( Web Application ) as Docker containers

``` 
Node.js server              Node.js server               Node.js server 
```

Dockerfile = **Contains instructions ( blueprint ) to build the Docker image**, defining what goes into the image and how the container should behave when it is running 

> 1. Use node base image to have node and npm available

> 2. Create /app folder and set as working directory 

> 3. Copy all necessary files from your host into the Docker image

> 4. Execute npm install to install dependencies 

> 5. Expose port 3000

> 6. Execute node command to start the server 

#### Source Code

```
Visual Studio Code
Explorer
Open Editors 
index.html
server.js
package.json
Dockerfile
```

Dockerfile
```dockerfile
FROM node:14

WORKDIR /app

COPY server.js .
COPY index.html .
COPY images ./images
COPY package.json .

RUN npm install

EXPOSE 3000

CMD ["node", "server.js"]
```

#### Docker Image 

> Standalone, executable package that includes everything needed to run the application 

> It is a snap shot of an environment that can be used to create and run containers 

#### Visual Studio Code
Terminal 
```
docker build -t myapp:1.0 . 
docker images | grep myapp 
> myapp                            1.0               b006...         20 seconds ago    864MB
```

> We can now easily run multiples instances of node.js server **without any conflicts** 

#### Start the containers... 

-p parameter = Bind the port on your local machine to a port inside the Docker container. 
To make an app running inside a container accessible from the outside world  

#### Visual Studio Code
Terminal 
```
docker run -p 3000:3000 myapp:1.0
> node app is listening on port 3000
> Request served by node app ( Refreshed Page )
> Request served by node app ( Refreshed Page )
```

Open in the Browser 

- localhost:3000 ( Web Application served by Node.js Backend )

```
TWN Career Quiz 
Your Custom Recommended Path 

Home           Best Courses            Fun Tutorials               About Tech World with Nana

TechWorld with Nana Programs 

    DEVOPS                           ...                    ...
   BOOTCAMP
    by TWN

Devops Bootcamp 

Finally learn with structured 
guided course, all Devops tools 
together

                            TechWorld with Nana. All Rights Reserved.
                            Follow us on: Linkedin | Twitter | Instagram 
```

#### Visual Studio Code
Terminal 
```
docker ps 
> CONTAINER ID    IMAGE        COMMAND                    CREATED            STATUS         PORTS                      NAMES 
  7b7d15...       myapp:1.0    "docker-entrypoints.s...   26 seconds ago     Up 26 sec...   0.0.0.0:3000->3000/tcp     interesting_williamson 
docker stop 7b7d15...
```

#### Start multiple instances ( Containers )

Docker Compose = Tool to simplify the process of defining and running multiple containers 

#### Source Code

```
Visual Studio Code
Explorer
Open Editors
> images 
> node_modules 
docker-compose.yaml 
index.html
server.js
package.json
Dockerfile
```

docker-compose.yaml
```yaml
version: '3'
services:
  app1:
    build: .
    environment:
      - APP_NAME=App1
    ports:
      - "3001:3000"

  app2:
    build: .
    environment:
      - APP_NAME=App2
    ports:
      - "3002:3000"

  app3:
    build: .
    environment:
      - APP_NAME=App3
    ports:
      - "3003:3000"
```

Access environment variables in the server.js application in node.js 

```
6 - const replicaApp = process.env.APP_NAME
12 - console.log{`Request served by ${replicaApp}`};
16 - console.log{`${replicaAPP} is listening on port ${port}`};
```

#### Source Code

```
Visual Studio Code
Explorer
Open Editors
> images 
> node_modules 
docker-compose.yaml 
index.html
server.js
package.json
Dockerfile
```

server.js
```javascript
const express = require('express');
const path = require('path');
const app = express();
const port = 3000;

const replicaApp = process.env.APP_NAME

const appName = process.env.APP_NAME

app.use('/images', express.static(path.join(__dirname, 'images')));

app.use('/', (req, res) => {
    res.sendFile(path.join(__dirname, 'index.html'));
    console.log{`Request served by ${replicaApp}`};
});

app.listen(port, () => {
    console.log{`${replicaAPP} is listening on port ${port}`};
});

```

We want to see, **which Node.js server is handling the request** 

We want to run all 3 containers on the **same machine** 

> So we need  to run then of **different ports**

```
8 - - "3001:3000"
15 - - "3002:3000"
22 -  - "3003:3000"
```

#### Source Code

```
Visual Studio Code
Explorer
Open Editors
> images 
> node_modules 
docker-compose.yaml 
index.html
server.js
package.json
Dockerfile
```

docker-compose.yaml
```yaml
version: '3'
services:
  app1:
    build: .
    environment:
      - APP_NAME=App1
    ports:
      - "3001:3000"

  app2:
    build: .
    environment:
      - APP_NAME=App2
    ports:
      - "3002:3000"

  app3:
    build: .
    environment:
      - APP_NAME=App3
    ports:
      - "3003:3000"
```

#### Visual Studio Code
Terminal 
```
docker-compose up --build -d 
> [+] building 3.4s (21/31)
-> [app1 internal] load build definition from Dockerfile
...
-> [app2 internal] load build definition from Dockerfile
...
-> [app3 internal] load build definition from Dockerfile
...

-> [app1] exporting to image 
-> -> exporting layers 
-> -> writing image sha256:...
-> -> naming to docker.io/Rodrigo\Full NGINX Tutorial Demo Project with Nodejs Docker\Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker
-> [app2] exporting to image 
-> -> exporting layers 
-> -> writing image sha256:...
-> -> naming to docker.io/Rodrigo\Full NGINX Tutorial Demo Project with Nodejs Docker\Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker
-> [app3] exporting to image 
-> -> exporting layers 
-> -> writing image sha256:...
-> -> naming to docker.io/Rodrigo\Full NGINX Tutorial Demo Project with Nodejs Docker\Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker
[+] Running 4/4
Network Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker_default Created
Container Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker-app1-1  Started
Container Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker-app2-1  Started
Container Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker-app3-1  Started 

docker ps
> CONTAINER ID     IMAGE              COMMAND                  CREATED      
  be5...           Full-NGINX...app1  "docker-entrypoint.s...  38 seconds ago
STATUS             PORTS                      NAMES 
Up 37 seconds      0.0.0.0:3001->3000/tcp     Full-NGINX...-app1-1
CONTAINER ID       IMAGE              COMMAND                  CREATED      
  846...           Full-NGINX...app2  "docker-entrypoint.s...  38 seconds ago
STATUS             PORTS                      NAMES 
Up 37 seconds      0.0.0.0:3002->3000/tcp     Full-NGINX...-app2-1
CONTAINER ID       IMAGE              COMMAND                  CREATED      
  ad3...           Full-NGINX...app3  "docker-entrypoint.s...  38 seconds ago
STATUS             PORTS                      NAMES 
Up 37 seconds      0.0.0.0:3003->3000/tcp     Full-NGINX...-app3-1
```

Open in the Browser 

- localhost:3001 ( Web Application served by Node.js Backend )

```
TWN Career Quiz 
Your Custom Recommended Path 

Home           Best Courses            Fun Tutorials               About Tech World with Nana

TechWorld with Nana Programs 

    DEVOPS                           ...                    ...
   BOOTCAMP
    by TWN

Devops Bootcamp 

Finally learn with structured 
guided course, all Devops tools 
together

                            TechWorld with Nana. All Rights Reserved.
                            Follow us on: Linkedin | Twitter | Instagram 
``` 

Open in the Browser 

- localhost:3002 ( Web Application served by Node.js Backend )

```
TWN Career Quiz 
Your Custom Recommended Path 

Home           Best Courses            Fun Tutorials               About Tech World with Nana

TechWorld with Nana Programs 

    DEVOPS                           ...                    ...
   BOOTCAMP
    by TWN

Devops Bootcamp 

Finally learn with structured 
guided course, all Devops tools 
together

                            TechWorld with Nana. All Rights Reserved.
                            Follow us on: Linkedin | Twitter | Instagram 
``` 


Open in the Browser 

- localhost:3003 ( Web Application served by Node.js Backend )

```
TWN Career Quiz 
Your Custom Recommended Path 

Home           Best Courses            Fun Tutorials               About Tech World with Nana

TechWorld with Nana Programs 

    DEVOPS                           ...                    ...
   BOOTCAMP
    by TWN

Devops Bootcamp 

Finally learn with structured 
guided course, all Devops tools 
together

                            TechWorld with Nana. All Rights Reserved.
                            Follow us on: Linkedin | Twitter | Instagram 
``` 

#### Visual Studio Code
Terminal 
```
docker logs be5...
> App1 is listening on port 3000
  Request served by App1 ( Refreshed Page )
  Request served by App1 ( Refreshed Page )

docker logs 846...
> App2 is listening on port 3000
  Request served by App2 ( Refreshed Page )
  Request served by App2 ( Refreshed Page )

docker logs ad3...
> App3 is listening on port 3000
  Request served by App3 ( Refreshed Page )
  Request served by App3 ( Refreshed Page )
```

## Configure NGINX Proxy 

> We want to have **1 entrypoint that forwards** to one of the backend servers

**ENGINX** 

> Load Balancer 

> Handle SSL encryption 

**Instal and Configure** NGINX 

> Load balance to the 3 web servers 

> Configure **HTTPS** 

Download and Install NGINX 

- https://nginx.org/en/download.html 

For Windows 

Command Prompt ( Terminal )
```
cd c:\
unzip nginx-1.27.3.zip
cd nginx-1.27.3
start nginx
nginx -v
whereis nginx 
nginx -V
cat /opt/homebrew/etc/nginx/nginx.conf
```
  
Visual Studio Command Palette  

>Shell Command: install 'code' command in PATH

```
Program Files\nginx-1.27.3\conf
```

#### Load balance to the 3 web servers

Directives 

**Worker_Processes** 

> Controls **how many parallel processes** Nginx spawns to handle client requests 

> Instead of using a new process for every incoming connection, Nginx uses work processes that **handle many connections** using a single-threaded event loop

> The value is the number of work processes Nginx should create 

> Each work process runs independently and can handle its own set of connections 

This configuration directly influences how well it can handle traffic ( Performance )

> Should be tunned according to the server's hardware ( CPU cores ) and expected traffic load 

#### Production environment 

```
4 Work Processes /  4 CPU Cores / Allows Nginx to efficiently use all CPU cores 
```

**auto** = Nginx automatically detects the number of CPU cores available on the server and starts a corresponding number of worker nodes

```
# Main context 
worker_processes auto;
```

#### worker_connections 

> Per worker process: **how many simultaneous connections** can be opened 

If you have **1 worker process**, you will be able to serve **1024 clients** 

If you have **2 worker processes**, you will be able to serve 2x1024 = *2048 clients** ( Increases memory usage )

nginx.conf
```conf
events {
    worker_connections 1024;
} 
```

#### http

**http** = configuration specific to HTTP and affecting all virtual servers 

Server Block 

> Defines how Nginx should handle requests for a particular domain or IP address 

- listen 

How to **listen** for connections 

> The IP address and port on which the server will accept requests 

Which domain or subdomain the configuration applies to 

How to route the requests 

- server_name

> Which domain or IP address this server block should respond to

- location 

> The root (/) URL, will apply to all requests unless more specific location blocks are defined 
 
Nginx as Proxy Server ( proxy_pass )

Pass the traffic to the Node.js application

> Tells Nginx to "pass" request to another server, making it act as a reverse proxy 

- upstream 

> Refers to servers that Nginx forward requests to 

> "upstream" name is based on the flow of data

> **Upstream servers** = Refers to traffic going from a client toward the source or higher-level infra, in this case application server

> **Downstream servers** = Traffic going back to the client is "downstream" 

> Upstream block defines a group of backend servers that will handle requests forwarded by Nginx 

localhost = 127.0.0.1 

port = 3001, 3002, 3003 ( Upstream of Nginx )

We want to forward info from the original client requests to the backend servers ( location )

> They provide useful information, which backend servers can use for logging or processing, etc

```
Original IP address
Original Host
Referrer Information 
Custom Headers
```

Common headers that can be forwarded 

nginx-conf
```conf
location / {
    # Pass client information to the backend
    proxy_set_header Host $host;
    proxy_set_header X-Real_IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X_Forwarded-Port $server_port;

    # Forward client browser and session information
    proxy_set_header User-Agent $http_user_agent;
    proxy_set_header Cookie $http_cookie;
    proxy_set_header Accept-Language $http_accept_language;
    proxy_set_header Referrer $http_referrer;

    # Forward client's authorization data
    proxy_set_header Authorization $http_authorization;

    # Custom headers ( optional )
    proxy_set_header X-Custom-Header "MyAppSpecificValue";
}
```

> When Nginx acts as a reverse proxy, the requests coming to the backend servers originate from Nginx, **not directly from the client**

> As a result, backend server would see the IP address of the Nginx server as the source of the request

We can tell Nginx to also pass the Original/Real IP address of the client 

- include ( MIME types )

We need to tell Nginx to **include the corresponding MIME types** in the "content-type" response header, when sending a file

> It helps the **client understand how to process or render the file**  

> mime.types is actually a file, that Nginx uses to map file extension to MIME types  

How does Nginx decide to which server to forward the request to ?

> Load balancing Algorithms

```
Round Robin ( Default )

Start from first server to the last one and it starts again from the first to the last one
```

```
least_conn

Request is sent to the server with the least number of active connections
```

#### Visual Studio Code
Terminal
```
nginx
nginx -v

> ...
--http-log-path=/...

cat ... ( path )

> ...
```

Open in the Browser 

- localhost:8080 ( Web Application served by Node.js Backend )

```
TWN Career Quiz 
Your Custom Recommended Path 

Home           Best Courses            Fun Tutorials               About Tech World with Nana

TechWorld with Nana Programs 

    DEVOPS                           ...                    ...
   BOOTCAMP
    by TWN

Devops Bootcamp 

Finally learn with structured 
guided course, all Devops tools 
together

                            TechWorld with Nana. All Rights Reserved.
                            Follow us on: Linkedin | Twitter | Instagram 
``` 

#### Visual Studio Code
Terminal
```
docker compose down

> [+] Running 0/3
Container Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker-app1-1  Stopping
Container Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker-app2-1  Stopping
Container Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker-app3-1  Stopping

> [+] Running 4/4
Container Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker-app1-1  Removed
Container Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker-app2-1  Removed
Container Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker-app3-1  Removed
Network Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker_default Created
```

Open in the Browser 

- localhost:8080 ( Web Application served by Node.js Backend )

```
                           502 Bad Gateway
                            nginx/1.27.3
```

#### Visual Studio Code
Terminal
```
docker-compose up --build

> [+] Running 4/4
Network Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker_default Created
Container Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker-app1-1  Created
Container Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker-app2-1  Created
Container Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker-app3-1  Created  

Attaching to app1-1, app2-1, app3-1
app1-1  | App1 is listening on port 3000
app2-1  | App2 is listening on port 3000
app3-1  | App3 is listening on port 3000
```

Open in Browser ( Refresh Page )

- localhost:8080 ( Web Application served by Node.js Backend )

```
TWN Career Quiz 
Your Custom Recommended Path 

Home           Best Courses            Fun Tutorials               About Tech World with Nana

TechWorld with Nana Programs 

    DEVOPS                           ...                    ...
   BOOTCAMP
    by TWN

Devops Bootcamp 

Finally learn with structured 
guided course, all Devops tools 
together

                            TechWorld with Nana. All Rights Reserved.
                            Follow us on: Linkedin | Twitter | Instagram 
``` 

#### Visual Studio Code
Terminal
```
> [+] Running 4/4
Network Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker_default Created
Container Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker-app1-1  Created
Container Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker-app2-1  Created
Container Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker-app3-1  Created  

Attaching to app1-1, app2-1, app3-1
app1-1  | App1 is listening on port 3000
app2-1  | App2 is listening on port 3000
app3-1  | App3 is listening on port 3000
app1-1  | Request served by App1
app2-1  | Request served by App2
```

- configure HTTPS - Secure Connection 

> HTTPS uses SSL to encrypt the data transmitted over the web 

> All communication between the client and the server is **encrypted** 

> Even if someone intercepts the data, they **can not read it**

> HTTPS is **standard on the internet** 

Obtain an SSL/TLS Certificate 

> SSL certificates enable encryption by using **public-key cryptography**

> When a user connects to a website via HTTPs, the **web server provides its SSL certificate, which contains a public key 

> The client **(browser)** uses this key to **stablish a secure**, encrypted session with the server 

Generate a **self-signed certificate**

Generated and signed by the server itself ( Locally )

> Useful for testing or internal sites, but **not recommended for production**

**CA-Signed** Certificate 

> Issued and Authenticated by a trusted certificate authority  

> CA **verifies the identity** of the organization requesting the certificate 

> Trusted by Browsers ( no warnings )

Generating SSl Certificate  ( Key pair )

> openssl = open-source tool to generate keys, certificates and managing secure connections  

Breakdown of commands 

**openssl req** 

> Initiates a certificate request generation process 

**-x509**

> Tells OpenSSL to output a certificate in this standard certificate format 

**-nodes**

> Tells OpenSSl not to encrypt the private key with a passphrase 

**-days 365** 

> Specifies validity period of the certificate, in this case 1 year 

**-newkey rsa:2048** 

> Creates a 2048-bit RSA key pair, RSA= most common public-key encryption algorithm 

Public key is shared with clients to encrypt data 

**-keyout nginx-selfsigned.key**

> Specifies the output file for the generated private-key, in this case "nginx-selfsigned.key"

Private key must be kept secretly. Its never shared publicly 

Used to decrypt data

**-out nginx-selfsigned.crt**

> Specifies the output file for the self signed certificate, in this case "nginx-selfsigned.crt"

> Contains the public key 

Open the Command Prompt ( The command generates a key-pair )

```
mkdir nginx-certs
cd nginx-certs
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout nginx-selfsigned.key -out nginx-selfsigned.crt
> Country Name (2 letter code) [AU]: BR 
...
> Locally Name (eg, city) []: Jaboticabal/SP
...
> Organization Name (eg, company) [Internet Widgits Pty Ltd]: RodrigoMvs123 
...

ls 
> nginx-selfsigned.crt       nginx-selfsigned.key

cat nginx-selfsigned.crt
> -----BEGIN CERTIFICATE----
...
  -----END CERTIFICATE-----
```

Configure HTTPS - for NGINX server 

> 443 is the **standard port for HTTPS** traffic, enabling SSL for secure communication 

> Configure the server to listen on port 443

> The configuration of http server will be secured with SSL

Look for the location path of the certificates

Command Prompt ( Terminal )
Terminal
```
Rodrigo /Users/Rodrigo/nginx-certs
pwd 
> ... 

```

Restart NGINX and see it in action 

> Nginx running with old configuration 

Reload or Restart Nginx 

Command Prompt ( Terminal )
Terminal
```
Rodrigo /Users/Rodrigo/nginx-certs
nginx -h
> Options: 
  -s   signal   : send signal to a master process: stop, quit, reopen, reload

nginx -s reload 
```

Open in Browser

- https://localhost ( Web Application served by Node.js Backend )


```
TWN Career Quiz 
Your Custom Recommended Path 

Home           Best Courses            Fun Tutorials               About Tech World with Nana

TechWorld with Nana Programs 

    DEVOPS                           ...                    ...
   BOOTCAMP
    by TWN

Devops Bootcamp 

Finally learn with structured 
guided course, all Devops tools 
together

                            TechWorld with Nana. All Rights Reserved.
                            Follow us on: Linkedin | Twitter | Instagram 
``` 

HTTP to HTTPS Redirection

> Client will be redirected even with an HTTP web address to an HTTPS endpoint server 

> 301 = Redirects the client ( browser ) to HTTPS using 301 status code, which indicates a permanent redirect 

Command Prompt ( Terminal ) 
Terminal
```
Rodrigo /Users/Rodrigo/nginx-certs
nginx -s reload
```

Open in Browser

- localhost:8080 ( Web Application served by Node.js Backend )

```
TWN Career Quiz 
Your Custom Recommended Path 

Home           Best Courses            Fun Tutorials               About Tech World with Nana

TechWorld with Nana Programs 

    DEVOPS                           ...                    ...
   BOOTCAMP
    by TWN

Devops Bootcamp 

Finally learn with structured 
guided course, all Devops tools 
together

                            TechWorld with Nana. All Rights Reserved.
                            Follow us on: Linkedin | Twitter | Instagram 
``` 

Redirected to 

- https://localhost ( Web Application served by Node.js Backend )

```
TWN Career Quiz 
Your Custom Recommended Path 

Home           Best Courses            Fun Tutorials               About Tech World with Nana

TechWorld with Nana Programs 

    DEVOPS                           ...                    ...
   BOOTCAMP
    by TWN

Devops Bootcamp 

Finally learn with structured 
guided course, all Devops tools 
together

                            TechWorld with Nana. All Rights Reserved.
                            Follow us on: Linkedin | Twitter | Instagram 
``` 

Command Prompt ( Terminal )
Terminal
```
Rodrigo /Users/Rodrigo/nginx-certs
nginx -s reload
```

Open in Browser

- localhost ( Web Application served by Node.js Backend )

```
TWN Career Quiz 
Your Custom Recommended Path 

Home           Best Courses            Fun Tutorials               About Tech World with Nana

TechWorld with Nana Programs 

    DEVOPS                           ...                    ...
   BOOTCAMP
    by TWN

Devops Bootcamp 

Finally learn with structured 
guided course, all Devops tools 
together

                            TechWorld with Nana. All Rights Reserved.
                            Follow us on: Linkedin | Twitter | Instagram 
``` 

Redirected to 

- https://localhost ( Web Application served by Node.js Backend )

```
TWN Career Quiz 
Your Custom Recommended Path 

Home           Best Courses            Fun Tutorials               About Tech World with Nana

TechWorld with Nana Programs 

    DEVOPS                           ...                    ...
   BOOTCAMP
    by TWN

Devops Bootcamp 

Finally learn with structured 
guided course, all Devops tools 
together

                            TechWorld with Nana. All Rights Reserved.
                            Follow us on: Linkedin | Twitter | Instagram 
``` 

nginx.conf
```conf
http {
    include mime.types;

    upstream nodejs_cluster {
        least_conn;
        server 127.0.0.1:3001;
        server 127.0.0.1:3002;
        server 127.0.0.1:3003;
    }

    server {
        listen 443 ssl;  
        server_name localhost; // Where Nginx proxy server is listening 
 
        ssl_certificate Users/Rodrigo/nginx-certs/nginx-$selfsigned.crt;    // Public Key
        ssl_certificate_key Users/Rodrigo/nginx-certs/nginx-selfsigned.key; // Private Key 



        location / {
            proxy_pass http://nodejs_cluster;
            proxy_set_header Host $host;    
            proxy_set_header X-Real-IP $remote_addr;
        }
    }

    server {
        listen 80;
        server_name localhost;

        location / {
            return 301 https://$host $request_uri;
        }
    }
}
```

#### Source Code

```
Visual Studio Code
Explorer
Open Editors
> images 
> node_modules 
docker-compose.yaml 
index.html
server.js
package.json
Dockerfile
nginx.conf
```

nginx.conf
```conf 
# Main context (this is the global configuration)
worker_processes 1;

events {
    worker_connections 1024;
}

http {
    include mime.types;

    # Upstream block to define the Node.js backend servers
    upstream nodejs_cluster {
        server 127.0.0.1:3001;
        server 127.0.0.1:3002;
        server 127.0.0.1:3003;
    }

    server {
        listen 443 ssl;  # Listen on port 443 for HTTPS
        server_name localhost;

        # SSL certificate settings
        ssl_certificate /Users/nana/nginx-certs/nginx-selfsigned.crt;
        ssl_certificate_key /Users/nana/nginx-certs/nginx-selfsigned.key;

        # Proxying requests to Node.js cluster
        location / {
            proxy_pass http://nodejs_cluster;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
    }

    # Optional server block for HTTP to HTTPS redirection
    server {
        listen 8080;  # Listen on port 80 for HTTP
        server_name localhost;

        # Redirect all HTTP traffic to HTTPS
        location / {
            return 301 https://$host$request_uri;
        }
    }
}
```

## Clean Up Resources 

Command Prompt ( Terminal )
Terminal
```
Rodrigo /Users/Rodrigo/nginx-certs
nginx -s stop

ps aux | grep nginx 
RodrigoMva123         33627   0.0    0.0    408636112      1440 s010  S+   1:28PM    0:00.00    grep nginx 
```

#### Visual Studio Code
Terminal
```
Rodrigo \Users\usuario\Desktop\Rodrigo\Full NGINX Tutorial Demo Project with Nodejs Docker\Full-NGINX-Tutorial-Demo-Project-with-Nodejs-Docker\index.html
docker ps
> CONTAINER ID   IMAGE    COMMAND   CREATED    STATUS    PORTS    NAMES   



