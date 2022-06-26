# Full Stack for Front-End Engineers, v2

## Servers

There are two ways of authentication in server:

- SSH (Secure Socket Shell)
- Password

SSH is made up of two parts - the _public key_ and _private key_.
The _private key_ stays on your computer and the _public key_ goes on the server.
The messages are encrypted using the public key and they can only be decrypted using the private key.

Creating an SSH key:

> cd ~/.ssh

> ssh-keygen

SSH into server:

> ssh root@YOUR_IP

### DNS Records

- Maps name to IP address -> A record
- Maps name to name -> CNAME

### Setting up a new user and disabling root

```bash
apt update
apt upgrade
adduser $USERNAME
usermod -aG sudo $USERNAME # add user to sudo group
su $USERNAME # switch user

# adding public key for this user
mkadir -p ~/.ssh
vi ~/.ssh/authorized_keys # paste public key

chmod 644 ~/.ssh/authorized_keys
sudo vi /etc/ssh/sshd_config # change PermitRootLogin to 'no'
sudo service sshd restart # restart the daemon
```

### Nginx

> sudo apt install nginx

> sudo service nginx start

This starts up a web server on the default port 80.

### Nginx Config

> /etc/nginx/nginx.conf

Nginx Redirect

```bash
location /help {
    return 301 https://developer.mozilla.org/en-US/;
}
```

Adding Subdomain

```bash
server {
    listen 80;
    listen [::]80; # IPV6 notation

    server_name test.madhavisthe.best;

    location / {
        proxy_pass http://localhost:3000;
    }
}
```

### Process Manager

- Keeps your application running
- Handles errors and restarts
- Can handle logging and clustering

```bash
sudo npm i -g pm2
pm2 start app.js # run node application
pm2 startup # setup auto restart
```

### Further Exploration

- Fail2ban - https://www.techrepublic.com/article/how-to-install-fail2ban-on-ubuntu-server-18-04/
- ExpressJS performance tips - http://expressjs.com/en/advanced/best-practice-performance.html

## Security

### Security Checklist

- SSH
- Firewalls
- Updates
- Two factor authentication
- VPN

#### Unattended Upgrades

> sudo apt install unattended-upgrades

#### Adding HTTPS to NGINX

https://certbot.eff.org/

**To add HTTP/2:**

```bash
sudo vi /etc/nginx/sites-available/default
```

Change to:

> listen 443 **http2** ssl
