# 🚀 NGINX Projects -- Complete Practical Guide

This repository contains **3 hands-on NGINX projects** covering:

1.  Serving Static Website
2.  NGINX as Reverse Proxy (Node.js)
3.  NGINX as Load Balancer
4.  Custom Error Pages
5.  Load Testing with Apache Benchmark

------------------------------------------------------------------------

# 📌 Prerequisites

-   Ubuntu Server (20.04/22.04)
-   Root or sudo access
-   Node.js & npm installed (for backend projects)
-   Domain configured (Optional)

------------------------------------------------------------------------

# 🔧 Install NGINX on Ubuntu

``` bash
sudo apt update
sudo apt install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx
sudo systemctl status nginx
```

Verify:

``` bash
http://your-server-ip
```

------------------------------------------------------------------------

# 🟢 Project 1 -- Serving Static Website Using NGINX

## Clone Project

``` bash
git clone https://github.com/AvikCodeCrafter/Nginx-Multi-Projects.git
cd nginx-static-website
```

## Step 1: Create Web Root

``` bash
sudo mkdir -p /var/www/static-site
sudo cp -r * /var/www/static-site
sudo chown -R www-data:www-data /var/www/static-site
```

## Step 2: Create Virtual Host

``` bash
sudo nano /etc/nginx/sites-available/static-site
```

Paste:

``` nginx
server {
    listen 80;
    server_name yourdomain.com;
    root /var/www/static-site;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

## Step 3: Enable Site

``` bash
sudo ln -s /etc/nginx/sites-available/static-site /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

✅ Your static website is now live.

------------------------------------------------------------------------

# 🛑 Custom Error Pages (404 & 500)

## Create Error Files

``` bash
sudo nano /var/www/static-site/404.html
sudo nano /var/www/static-site/500.html
```

## Update Server Block

``` nginx
error_page 404 /404.html;
error_page 500 502 503 504 /500.html;

location = /404.html {
    internal;
}

location = /500.html {
    internal;
}
```

Reload NGINX:

``` bash
sudo systemctl reload nginx
```

------------------------------------------------------------------------

# 🔁 Project 2 -- NGINX as Reverse Proxy

## Clone Project

``` bash
git clone https://github.com/jaiswaladi246/nginx-node-proxy.git
cd nginx-node-proxy
```

## Setup Frontend

``` bash
sudo mkdir -p /var/www/frontend
sudo cp -r frontend/* /var/www/frontend/
sudo chown -R www-data:www-data /var/www/frontend
```

## Configure NGINX

``` bash
sudo nano /etc/nginx/sites-available/nginx-node-proxy
```

``` nginx
server {
    listen 80;
    server_name yourdomain.com;

    root /var/www/frontend;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location /api/ {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    location /socket.io/ {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

Enable:

``` bash
sudo ln -s /etc/nginx/sites-available/nginx-node-proxy /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

## Start Backend

``` bash
cd backend
npm init -y
npm install express cors socket.io
node index.js
```

✅ Reverse proxy is configured successfully.

------------------------------------------------------------------------

# ⚖️ Project 3 -- NGINX as Load Balancer

## Clone Project

``` bash
git clone https://github.com/jaiswaladi246/nginx-loadbalancer.git
cd nginx-loadbalancer
```

## Configure NGINX

``` bash
sudo nano /etc/nginx/sites-available/nginx-loadbalancer
```

``` nginx
upstream backend_apis {
    least_conn;
    server localhost:3001;
    server localhost:3002;
}

server {
    listen 80;
    server_name yourdomain.com;

    location /api/ {
        proxy_pass http://backend_apis;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

Enable & Reload:

``` bash
sudo ln -s /etc/nginx/sites-available/nginx-loadbalancer /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

## Start Backends

Terminal 1:

``` bash
cd backend1
npm init -y
npm install express
node index.js
```

Terminal 2:

``` bash
cd backend2
npm init -y
npm install express
node index.js
```

------------------------------------------------------------------------

# 📊 Load Testing

Install Apache Benchmark:

``` bash
sudo apt install apache2-utils -y
```

Run Test:

``` bash
ab -n 100 -c 10 http://yourdomain.com/api/
```

------------------------------------------------------------------------

# 🧠 Key Concepts Covered

-   Virtual Hosts
-   Reverse Proxy
-   WebSocket Proxying
-   Load Balancing (least_conn)
-   Custom Error Handling
-   Traffic Simulation

------------------------------------------------------------------------

# 🏁 Conclusion

This guide provides real-world NGINX production use cases:

✔ Static hosting\
✔ API reverse proxy\
✔ WebSocket handling\
✔ Horizontal scaling with load balancing\
✔ Performance testing

------------------------------------------------------------------------

# 👨‍💻 Author

Avik Banerjee\
DevOps \| Cloud \| SRE \| Automation

------------------------------------------------------------------------
