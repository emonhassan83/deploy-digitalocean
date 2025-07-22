Here is the updated document with the necessary Nginx commands and a reference to the tutorial for configuring Nginx:

---

# Nginx Configuration and Commands
(remmember use conf.d when host any backend and use sites-available when host any frontend)
## Overview
This document provides essential commands for managing the Nginx web server. For a detailed guide on installing and configuring Nginx, refer to the [DigitalOcean Nginx Installation Guide](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04).

## Basic Nginx Commands

1. **Check Nginx Status:**
   - To check the status of the Nginx service:
     ```
     sudo systemctl status nginx
     ```

2. **Start Nginx:**
   - To start the Nginx service:
     ```
     sudo systemctl start nginx
     ```

3. **Stop Nginx:**
   - To stop the Nginx service:
     ```
     sudo systemctl stop nginx
     ```

4. **Restart Nginx:**
   - To restart the Nginx service (useful after making configuration changes):
     ```
     sudo systemctl restart nginx
     ```

5. **Reload Nginx:**
   - To reload Nginx without interrupting existing connections (useful for applying changes to the configuration):
     ```
     sudo systemctl reload nginx
     ```

6. **Enable Nginx on Boot:**
   - To enable the Nginx service to start on system boot:
     ```
     sudo systemctl enable nginx
     ```

7. **Disable Nginx on Boot:**
   - To disable the Nginx service from starting on system boot:
     ```
     sudo systemctl disable nginx
     ```

8. **Test Nginx Configuration:**
   - To test the Nginx configuration for syntax errors:
     ```
     sudo nginx -t
     ```

9. **View Nginx Logs:**
   - To view Nginx access logs:
     ```
     sudo tail -f /var/log/nginx/access.log
     ```
   - To view Nginx error logs:
     ```
     sudo tail -f /var/log/nginx/error.log
     ```




#### Configure nginx with backend:
1. create a file called api.conf in conf.d folder

then go to the files using editor like nano api.conf 
then write a config ex:
```javascript
server {
    listen 80;
    server_name example.com www.example.com; 

    location / {
        proxy_pass http://159.223.184.53:1000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

#### Configure nginx with socket:
1.create a file called socket.conf in conf.d folder

then go to the files using editor like nano socket.conf 
then write a config ex:

```javascript
server {
    listen 80;
    server_name socket.example.com www.socket.socket.com; //here your domain 

    location / {
        proxy_pass http://167.99.204.29:9001; //here your socket host and port
        
        # Add the following headers for WebSocket support
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        
        # Add timeout settings for WebSocket connections
        proxy_read_timeout 86400;
        proxy_send_timeout 86400;
    }
}
```

then check and reload nginx 

**Test Nginx Configuration:**
   - To test the Nginx configuration for syntax errors:
     ```bash
     sudo nginx -t
     ```
 **Reload Nginx:**
   - To reload Nginx without interrupting existing connections (useful for applying changes to the configuration):

  ```bash
    sudo systemctl reload nginx
  ```


#### Configure nginx with frontend and dashboard:

1. config nginx in sites-available folder 
like example.conf

2. Create a Symbolic Link to sites-enabled:
To enable this configuration, you need to create a symbolic link in the /etc/nginx/sites-enabled/ folder that points to your configuration in sites-available/.

Run the following command to create the symlink:

```bash
sudo ln -s /etc/nginx/sites-available/example.conf /etc/nginx/sites-enabled/
```

3. Test the NGINX Configuration
After adding the symlink, test if the NGINX configuration is correct by running:

```bash
sudo systemctl reload nginx

```

If the test passes, you should see something like:
```bash
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
4. Reload NGINX
To apply the changes, reload NGINX with the following command:

```bash
sudo systemctl reload nginx
```


8. Set nginx bodysize   go to
 ```bash
   nano ../etc/nginx/nginx.conf
```
then set this code under http
```
client_max_body_size 500M;
```
## INSTALL AND CONFIGURE PM2


follow their documentation to install and configure pm2 as well:https://pm2.keymetrics.io/
