<h1 align="center">Squid HTTPS Proxy Setup</h1>

<p align="center">
  <em>This guide provides instructions for setting up Squid as an HTTPS proxy on a vps server. Squid is a caching proxy that supports various protocols. This guide specifically focuses on HTTPS proxying for improved security and privacy.</em>
</p>
<hr>

## Requirements 

Before you begin, ensure you have the following:
- A server running a Linux distribution (Ubuntu/Debian is used in this guide)
- Root or sudo access to the server
- Basic understanding of terminal commands

## Installation

To install Squid and necessary utilities, run the following commands:

```bash
sudo apt update
sudo apt install squid apache2-utils
```
## Configuration

To configure Squid with basic authentication and safe port definitions, follow these steps:

1. **Edit the Squid configuration file** Located at `/etc/squid/squid.conf` with your preferred text editor, such as nano:

```bash
sudo nano /etc/squid/squid.conf
```
Replace the file's full text with the following configuration code. This setup includes ACL definitions for local networks, safe ports, SSL ports, and the authentication parameters:

```squid
acl localnet src 0.0.0.0/0.0.0.0
acl SSL_ports port 443
acl Safe_ports port 80 21 443 70 210 1025-65535 280 488 591 777
acl CONNECT method CONNECT

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm "Squid proxy-caching web server"
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive off

acl auth_users proxy_auth REQUIRED

http_access allow localnet
http_access allow localhost
http_access allow auth_users
http_access deny all

http_port 3128

coredump_dir /var/spool/squid
```
2. **Allow connections to the proxy port** After configuring Squid, complete the setup by allowing traffic through the proxy port and restarting the Squid service to apply your changes:

```bash
sudo ufw allow 3128
sudo systemctl restart squid
```

3. **Create user & password for authentication** you'll need to create a password file for Squid authentication. Replace `username` with your desired username:

```bash
sudo htpasswd -c /etc/squid/passwd username
```

After entering, you need to set up a password, enter and confirm a password for the user. You can create a new user by simply changing the `username` and setting a `password`.

## Usage

To use the proxy, configure your browser to use the following settings:

- **Proxy server:** `ip` your server ip
- **Port:** `3128` Default port
- **Username and password:** Use the credentials from the password and username you created earlier.
- **Demo:** `111.1.1.111:3128:demopass:123456`

This configuration will route your browser traffic through the Squid proxy, requiring authentication with the username and password you've set up.

## Author 📝

- Name: Bisnu Ray
- Telegram: [@SmartDev](https://t.me/itsSmartDev)

Feel free to reach out if you have any questions or feedback.
