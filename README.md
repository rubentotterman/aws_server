 Secure a Linux Server Project

Project Description
This project demonstrates how to secure a cloud Linux server (Ubuntu 22.04) step-by-step, using best practices for SSH hardening, firewall configuration, and automated intrusion prevention.

The project was performed manually, with full understanding of each security control applied. It is intended as a real-world showcase of Linux server administration and DevOps skills.



🔹 Steps Completed

1. Server Setup
- Launched a cloud VPS instance (AWS Lightsail)
- OS: Ubuntu 22.04 LTS
- Accessed via public IP using an SSH `.pem` private key

```bash
ssh -i ~/Desktop/LightsailDefaultKey-eu-north-1.pem ubuntu@<your-server-public-ip>
```

2. Created a New User
- Created a new user `ruben`
- Set a strong password (for sudo authentication)

```bash
sudo adduser ruben
```

3. Configured SSH Key Authentication for New User
- Created `.ssh` directory and copied authorized keys:

```bash
sudo mkdir /home/ruben/.ssh
sudo cp ~/.ssh/authorized_keys /home/ruben/.ssh/
sudo chown -R ruben:ruben /home/ruben/.ssh
sudo chmod 700 /home/ruben/.ssh
sudo chmod 600 /home/ruben/.ssh/authorized_keys
```

4. Granted Sudo Privileges to New User
- Added ruben to the `sudo` group:

```bash
sudo usermod -aG sudo ruben
```


5. Hardened SSH Configuration
- Edited SSH daemon configuration file:

```bash
sudo nano /etc/ssh/sshd_config
```

- Made the following changes:
  ```
  PermitRootLogin no
  PasswordAuthentication no
  DenyUsers ubuntu
  ```

- Reloaded SSH service:

```bash
sudo systemctl reload sshd
```


6. Installed and Configured Firewall (UFW)
- Installed UFW firewall:

```bash
sudo apt update
sudo apt install ufw
```

- Allowed SSH connections:

```bash
sudo ufw allow OpenSSH
```

- Enabled UFW:

```bash
sudo ufw enable
```

- Confirmed status:

```bash
sudo ufw status verbose
```


7. Installed Fail2Ban for Intrusion Prevention
- Installed Fail2Ban:

```bash
sudo apt install fail2ban
```

- Enabled and started the service:

```bash
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

- Created a custom configuration:

```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local
```

- Verified SSH jail was active:

```bash
sudo fail2ban-client status sshd
```


---

🔹 Final Result
- Server access secured via SSH keys only
- Root login disabled
- Default `ubuntu` user login disabled
- UFW firewall active (SSH only allowed)
- Fail2Ban protecting against brute-force attacks

---

🔹 Technologies Used
- Ubuntu 22.04 LTS
- OpenSSH
- UFW (Uncomplicated Firewall)
- Fail2Ban
- AWS Lightsail

---

📈 Future Improvements
- Enable automatic security updates
- Set up 2FA (two-factor authentication) for SSH login
- Change default SSH port
- Configure monitoring and alerting
- Schedule regular backups / snapshots

---


This project demonstrates complete initial hardening of a Linux cloud server.

Further enhancements (patch automation, advanced monitoring, infrastructure as code) are planned for next phases.


