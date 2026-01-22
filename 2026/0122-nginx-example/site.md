---
category: Code Snippets
slug: ngix-ssl
title: nginx ssl
date: 2026-01-22
tags: []
page: null
draft: false
---

I'm putting this page here mainly just for my reference. 

1) Run service on pm2

2) Create new file /etc/nginx/sites-available/[tendril.cc]:

```
server {
    listen 80;
    listen [::]:80;
    server_name tendril.cc www.tendril.cc;

    location / {
        proxy_pass http://127.0.0.1:6101;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
      }
  }
```

3) run these commands on command line:

```
sudo ln -sf /etc/nginx/sites-available/[tendril.cc]() /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
sudo apt update && sudo apt install -y certbot python3-certbot-nginx
sudo certbot --nginx -d tendril.cc
```