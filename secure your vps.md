Check UFW status (detailed)
```bash
ufw status verbose
```
If UFW is NOT active
```bash
ufw allow OpenSSH
ufw allow 80
ufw allow 443
ufw enable
```
View firewall rules with numbers
```bash
ufw status numbered
```
Remove unwanted port (example: port 5000)
```bash
ufw delete allow 5000/tcp
```

🛡️ FAIL2BAN (BRUTE-FORCE PROTECTION)
Check if Fail2Ban is installed
```bash
fail2ban-client -V
```
If version is shown → installed ✅
If command not found → install it (next step).

Install Fail2Ban (if not installed)
```bash
apt install fail2ban -y
```
Enable and start:
```bash
systemctl enable fail2ban
systemctl start fail2ban
```
Check service:
```bash
systemctl status fail2ban
```

Check SSH protection (IMPORTANT)
```bash
fail2ban-client status sshd
```
Expected output:

Status for the jail: sshd
Currently banned: 0
Total banned: 0
This means SSH brute-force protection is active 🔐

If SSH jail is missing (rare case)

Restart Fail2Ban:
```bash
systemctl restart fail2ban
```
Check again:
```bash
fail2ban-client status sshd
```
✅ FINAL SECURITY CHECKLIST
 UFW enabled
 Incoming traffic denied by default
 Only ports 22, 80, 443 open
 Fail2Ban installed
 SSH protected from brute-force attacks

🧪 QUICK VERIFICATION COMMANDS
```bash
ufw status verbose
systemctl status fail2ban
fail2ban-client status sshd
```
🎉 DONE
Your server is now protected against:
Brute-force SSH attacks
Status: SECURE & PRODUCTION-READY ✅








ufw status verbose
fail2ban-client status sshd
