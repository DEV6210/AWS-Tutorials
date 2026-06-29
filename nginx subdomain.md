Step 8: Create Nginx config
```
sudo nano /etc/nginx/sites-available/api.mywealthyfuturecsp.com
```
Paste this:
```
server {
    listen 80;
    server_name api.mywealthyfuturecsp.com;

    location / {
        proxy_pass http://127.0.0.1:5010;

        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
Save and exit.

Step 9: Enable the site
```
sudo ln -sf /etc/nginx/sites-available/api.mywealthyfuturecsp.com /etc/nginx/sites-enabled/
```
Step 10: Test Nginx
```
sudo nginx -t
```
If you get:

syntax is ok
test is successful

then reload:

sudo systemctl reload nginx
Step 11: Check HTTP

Open:
```
http://api.mywealthyfuturecsp.com
```
If your API opens, proceed to SSL.

Step 12: Install SSL
```
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
```
Run:
```
sudo certbot --nginx -d api.mywealthyfuturecsp.com
```
When prompted:

Redirect HTTP to HTTPS?

Choose:

2

After completion, test:
```
https://api.mywealthyfuturecsp.com
```
You should see a 🔒 secure connection.

If Certbot fails

Run these commands and send me the output:
```
sudo nginx -t
sudo systemctl status nginx
sudo certbot --nginx -d api.mywealthyfuturecsp.com
```
I'll help you fix any issue.

One important question:

Is your Node.js backend listening on 127.0.0.1:5010 (localhost only) or 0.0.0.0:5010 (all interfaces)?

If you're not sure, run:
```
sudo ss -tulpn | grep 5010
```
