
Verify DNS from VPS:
```bash
ping ezycore.cloud


🌐 CHECK / INSTALL NGINX

Check if Nginx is installed:

nginx -v


If not installed:

apt update
apt install nginx -y
systemctl start nginx
systemctl enable nginx


Verify:

systemctl status nginx

🔥 FIREWALL (UFW) SETUP

Allow required ports:

ufw allow OpenSSH
ufw allow 80
ufw allow 443
ufw enable


Check:

ufw status

⚙️ NGINX CONFIGURATION (MAIN STEP)

Create config file:

nano /etc/nginx/sites-available/ezycore.cloud


Paste the following:

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


Enable site:

ln -s /etc/nginx/sites-available/ezycore.cloud /etc/nginx/sites-enabled/
nginx -t
systemctl reload nginx

🟢 NODE.JS CONFIGURATION

Make sure Node.js listens only on localhost:

app.listen(5000, '127.0.0.1');


Start backend using PM2:

pm2 start app.js --name backend-api
pm2 save
pm2 startup


Check status:

pm2 status

🚫 BLOCK DIRECT PORT ACCESS

Remove public access to port 5000:

ufw delete allow 5000/tcp
ufw status

🔐 SSL (HTTPS) SETUP

Install Certbot:

apt install certbot python3-certbot-nginx -y


Generate SSL:

certbot --nginx -d ezycore.cloud -d www.ezycore.cloud


Test auto-renew:

certbot renew --dry-run

🧪 FINAL TEST

❌ These must NOT work:

http://IP:5000
http://ezycore.cloud:5000


✅ This MUST work:

https://ezycore.cloud
