
Verify DNS from VPS:
```bash
ping ezycore.cloud
```

🌐 CHECK / INSTALL NGINX

Check if Nginx is installed:
```bash
nginx -v
```

If not installed:
```bash
apt update
apt install nginx -y
systemctl start nginx
systemctl enable nginx
```

Verify:
```bash
systemctl status nginx
```
🔥 FIREWALL (UFW) SETUP

Allow required ports:
```bash
ufw allow OpenSSH
ufw allow 80
ufw allow 443
ufw enable
```

Check:
```bash
ufw status
```
⚙️ NGINX CONFIGURATION (MAIN STEP)

Create config file:
```bash
nano /etc/nginx/sites-available/ezycore.cloud
```

Paste the following:
```bash
server {
    listen 80;
    server_name ezycore.cloud www.ezycore.cloud;

    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

Enable site:
```bash
ln -s /etc/nginx/sites-available/ezycore.cloud /etc/nginx/sites-enabled/
nginx -t
systemctl reload nginx
```
🟢 NODE.JS CONFIGURATION

Make sure Node.js listens only on localhost:

app.listen(5000, '127.0.0.1');


Start backend using PM2:
```bash
pm2 start app.js --name backend-api
pm2 save
pm2 startup
```

Check status:
```bash
pm2 status
```
🚫 BLOCK DIRECT PORT ACCESS

Remove public access to port 5000:
```bash
ufw delete allow 5000/tcp
ufw status
```
🔐 SSL (HTTPS) SETUP

Install Certbot:
```bash
apt install certbot python3-certbot-nginx -y
```

Generate SSL:
```bash
certbot --nginx -d ezycore.cloud -d www.ezycore.cloud
```

Test auto-renew:
```bash
certbot renew --dry-run
```
🧪 FINAL TEST

❌ These must NOT work:

http://IP:5000
http://ezycore.cloud:5000


✅ This MUST work:

https://ezycore.cloud
