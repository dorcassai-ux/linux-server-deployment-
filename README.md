# Linux Server Deployment

Deploys and secures an nginx web server on an Ubuntu VM (VirtualBox).
Built for the Codomax Digital internship, Module 02.

## Features
- Ubuntu 26.04 LTS virtual machine running nginx, enabled at boot
- SSH key login using an ED25519 key pair
- UFW firewall allowing SSH (22) and HTTP (80)

## Setup
1. Update the system:
   `sudo apt update && sudo apt upgrade -y`
2. Install the SSH server:
   `sudo apt install openssh-server -y`
3. Generate a key pair and authorise the public key:
   ```
   ssh-keygen -t ed25519 -f ~/.ssh/my-keypair
   cat ~/.ssh/my-keypair.pub >> ~/.ssh/authorized_keys
   chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys
   ```
4. Configure the firewall (allow SSH before enabling it):
   ```
   sudo ufw allow 22
   sudo ufw allow 80
   sudo ufw enable
   ```
5. Install and start nginx:
   ```
   sudo apt install nginx -y
   sudo systemctl enable --now nginx
   ```

## Verification
- `sudo ufw status` shows the firewall active with ports 22 and 80 allowed
- `ssh -i ~/.ssh/my-keypair user@localhost` logs in without a password
- `systemctl status nginx` shows active (running)
- `curl http://localhost` returns a response from nginx

![Firewall status](screenshots/07-openssh-and-firewall.png)
![SSH key login and nginx status](screenshots/08-ssh-key-login-and-nginx.png)
![curl test](screenshots/09-curl-test.png)

## Documentation
Full Deployment Documentation with screenshots: [doc/Linux_Server_Deployment.pdf](doc/Linux_Server_Deployment.pdf)


## Note
The same steps apply to an AWS EC2 instance, where a security group
replaces the firewall.
