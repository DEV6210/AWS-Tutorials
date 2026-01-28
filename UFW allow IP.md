1️⃣ UFW allow (tumne bola)
ufw allow from 103.215.227.116 to any port 22 proto tcp

2️⃣ Fail2Ban whitelist (MOST IMPORTANT)
```bash
nano /etc/fail2ban/jail.d/sshd.local
```
```bash
[sshd]
enabled = true
ignoreip = 127.0.0.1/8 ::1 YOUR_IP
```

Restart:
```bash
systemctl restart fail2ban
```
