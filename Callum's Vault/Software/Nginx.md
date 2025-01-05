# Overview

Nginx is a web server, reverse proxy, content cache, load balancer, TCP/UDP proxy server, and mail proxy server.

## Quick important points

There are 4 main components to Nginx, these are:
- Available sites - This is the sites that are ready to be accessed. It is stored at `/etc/nginx/sites-available/`.
- Enabled sites - This is a list of sites that are enabled and publicly available to access. It is stored in `/etc/nginx/sites-enabled/`.
- Site configuration files - This is where the config files for each site is stored. Each site is given its own `.conf` file. These files allow you to direct traffic based on its request. It is stored in `/etc/nginx/conf.d/`.
- Webpages - This is where any local webpages are stored and are accessed by Nginx. On Debian distributions like Ubuntu, it is typically stored in `/var/www/your-domains-here.co.uk`.

Nginx creates a user when it is installed called `www-data` or `nginx`. It uses these accounts to access the 4 components above so ensure that it has access to these paths if you are having issues.

Be sure that Nginx is accessible from outside your local host by port forwarding the required ports. You will only most likely need ports `80` (HTTP) and `443` (HTTPS).

# How to install

## Ubuntu

### 1. Installing Nginx

```Shell
sudo apt install nginx
```

### 2. Adjust your firewall

If you are using a firewall, you need to allow Nginx to run through the firewall using the following command:

```Shell
sudo ufw app list
```

Find the Nginx that you wish to use from the list:
- Nginx Full
- Nginx HTTP
- Nginx HTTPS

Paste the following command choosing whichever one you are going to use:

```Shell
sudo ufw allow 'Nginx Full'
```

You can check if it was successful using the following command:

```Shell
sudo ufw status
```

### 3. Check that Nginx is running

You can check if Nginx is running using the following command:

```Shell
systemctl status nginx
```

### 4. Browse to your IP

If all has gone well, browsing to your IP should bring up the default NGINX webpage:

```Browser
http://your_server_ip
```

# How to create a web page with a domain

## Create a folder with your domain name

After you have installed Nginx, create a folder in the following location, and change your domain name to the name you have registered

```Shell
mkdir /var/www/domain_name.co.uk
```

You can place your website files by creating a html folder and placing your html and css files inside of it

```Shell
mkdir /var/www/domain_name.co.uk/html
```

Your structure should look like so:
/var/www/domain_name.co.uk/
└── html
	├── index.html
	├── css
		└── main.css

## Correct ownership of folder

With the folder created, change the owner of the file to the owner `www-data`. This is the account created by Nginx to read and access files.

```Shell
sudo chown -R www-data:www-data /var/www/domain_name.co.uk
```

## Make the domain available

Create the following file in the correct directory using the following command:

```Shell
sudo nano /etc/nginx/sites-available/domain_name.co.uk
```

Inside that file, paste the following:

```Text
server {
        listen 80;
        listen [::]:80;

        root /var/www/domain_name.co.uk/html;
        index index.html index.htm index.nginx-debian.html;

        server_name domain_name.co.uk www.domain_name.co.uk;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

## Enable the domain

With the domain now able to be public, you must now enable it. Create a symbolic link to the `sites-enabled` folder to enable the domain:

```Shel
sudo ln -s /etc/nginx/sites-available/domain_name.net /etc/nginx/sites-enabled/
```

## Restart the Nginx service

Restart the service by entering the following command. This should be ran anytime ***Nginx*** change has been made:

```Shell
sudo systemctl restart nginx
```

## Check syntax errors

After restarting the service and Nginx has successfully applied the changes, check that the config files have successfully applied and have no errors by using the following command:

```Shell
sudo nginx -t
```

# Setting up a subdomain

## Create folders for the subdomains (website hosting)

Create your folders in the same location that hosts your main domain `/var/www/sub.domain_name.co.uk`

For example, it should look like this:

- /var/www/callumwellard.co.uk - main domain of callumwellard.co.uk
- /var/www/webapp.callumwellard.co.uk - Subdomain for a webapp

### Correct ownership of the subdomains

Ensure that Nginx can access these folders by changing ownership of the folders to `www-data`; the account used by Nginx.

```Shell
sudo chown -R www-data:www-data /var/www/sub.domain_name.co.uk
```

With the example above, it should look like so:

```Shell
sudo chown -R www-data:www-data /var/www/webapp.callumwellard.co.uk
```

## Make the subdomains available

Create the configuration files for your subdomains in the following location `/etc/nginx/sites-available/` with the name of the subdomain.

```Shell
sudo nano /etc/nginx/sites-available/sub.domain_name.co.uk
```

Inside the file, paste the following and make sure to replace `sub.domain_name.co.uk` with your own subdomain:
## Enable the domain

Enable the site and make it live by creating a symbolic link in `/etc/nginx/sites-enabled`:

```Shell
sudo ln -s /etc/nginx/sites-available/sub.domain_name.co.uk /etc/nginx/sites-enabled/
```

## Editing domain configuration files

Inside the `sites-available` file, paste the following:

```conf
server {
        listen 80;
        listen [::]:80;

		server_name sub.domain_name.co.uk www.sub.domain_name.co.uk;
        index index.html index.htm index.nginx-debian.html;

        location / {
		    proxy_pass http://IP.GOES.HERE:PORT;
		    proxy_set_header Host $host;
		    proxy_set_header X-Real-IP $remote_addr;
		    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		    proxy_set_header X-Forwarded-Proto $scheme;

			root /var/www/domain.co.uk/html;
			try_files $uri $uri/ =404;
			# (Optional - for serving static webpages)
		}
}
```

> [!Warning]
> Be sure to change the IP address, port number, and the domain names.

For an explanation of the above, please click the collapsible section below:

> [!NOTE]- Nginx configuration explained:
> **listen 80;**
> **listen [::]:80;**
> What ports this domain should listen on
> 
> **server_name sub.domain_name.co.uk www.sub.domain_name.co.uk;**
> Specifies the domain names (or hostnames) that Nginx will respond to.
> 
> **index index.html index.htm index.nginx-debian.html;**
> Specifies the order in which Nginx should look for index files when a client requests this domain. (Optional - for serving static webpages)
> 
> **proxy_pass http://IP.GOES.HERE:PORT;**
> This specifies the destination for the proxy. Nginx will forward all incoming requests to the specified IP address and port.
> 
> **proxy_set_header Host $host;**
> This sets the Host header in the proxied request to be the same as the original Host header from the client’s request ($host refers to the domain or IP address the client requested).
> 
> **proxy_set_header X-Real-IP $remote_addr;**
> This sets the X-Real-IP header to the client’s original IP address ($remote_addr), so the backend server knows the true client IP, not the IP of the Nginx server.
> 
> **proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;**
> This appends the client’s IP address to the X-Forwarded-For header. The X-Forwarded-For header is commonly used to track the chain of proxy servers through which the request has passed. Nginx adds the client IP (or the last proxy’s IP) to this header.
> 
> **proxy_set_header X-Forwarded-Proto $scheme;**
> This sets the X-Forwarded-Proto header to indicate the protocol (HTTP or HTTPS) used in the original client request. $scheme is a variable in Nginx that automatically resolves to either http or https based on the connection.
> 
> **root /var/www/domain.co.uk/html;**
> This sets the directory where your static web pages will be located.
> 
> **try_files $uri $uri/ =404;**
> This configuration is often used for static websites or content that is directly served from the file system. It tells Nginx to:
> 1) Try to serve the exact file matching the URL.
> 2) If no file is found, check if the URL matches a directory and try to serve an index file (like index.html).
> 3) If neither a file nor a directory exists, return a 404 error.
## Check syntax errors

After editing the file, check that the config files has no errors by using the following command:

```Shell
sudo nginx -t
```

## Restart the Nginx service

Restart the service by entering the following command. This should be ran ***anytime Nginx*** change has been made:

```Shell
sudo systemctl restart nginx
```

Your domain should now be publicly accessible via a reverse proxy.
# Setting up HTTPS with LetsEncrypt/Certbot

## Installing Certbot and the Nginx package

(Ubuntu) - Install Certbot using the following commands:

```Shell
sudo apt-get install certbot python3-certbot-nginx -y
```

## Run certbot for each domain

```Shell
sudo certbot certonly --agree-tos --email myemail@email.com -d domain_name.co.uk
sudo certbot certonly --agree-tos --email myemail@email.com -d sub.domain_name.co.uk
```

A message will appear asking how you would like to authenticate. Select the option with `Nginx Web Server plugin (nginx)`.

> [!Warning]
> If you do not see the option for Nginx Web Server Plugin, then you didn't install the package above.
> Please run this command: `sudo apt-get install certbot python3-certbot-nginx -y`
## Reedit your server blocks to enable SSL

```conf
server {
    listen 80;
    listen [::]:80;
    server_name sub.domain.co.uk www.sub.domain.co.uk;
    return 301 https://sub.domain.co.uk$request_uri;
    # Returns all HTTP requests and requests it as HTTPS instead
}

server {
    listen 443 ssl;
    listen [::]:443 ssl;

    server_name sub.domain.co.uk;

    ssl_certificate /etc/letsencrypt/live/sub.domain.co.uk/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/sub.domain.co.uk/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

    location / {
        proxy_pass http://IP.GOES.HERE:PORT;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # For WebSocket support, if needed
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

		root /var/www/domain.co.uk/html;
		try_files $uri $uri/ =404;
		# (Optional - for serving static webpages)
    }
}
```

## Check syntax errors

After editing the domain to enable SSL, check that the config file has no errors by using the following command:

```Shell
sudo nginx -t
```
## Restart the Nginx service

If there are no errors, restart the service by entering the following command. This should be ran ***anytime Nginx*** change has been made:

```Shell
sudo systemctl restart nginx
```

## Automatically renew all certs for all domains

You can run the following command to run Certbot for all domains:

```bash
EMAIL="certbot@callum.mozmail.com"

# Path to the directory containing the Nginx configuration files
NGINX_CONF_DIR="/etc/nginx/sites-available"

# Loop through each file in the directory  
for FILE in "$NGINX_CONF_DIR"/*; do  
  # Extract the filename (domain name)
  DOMAIN=$(basename "$FILE")

  # Run the Certbot command for the domain
  echo "Running certbot for domain: $DOMAIN"
  sudo certbot certonly --agree-tos --email "$EMAIL" -d "$DOMAIN"
done
```

# Common troubleshooting

## Finding log file

The log file for Nginx is located in:
`/var/log/nginx/error.log`

You can use `cat` to show the output like so:
`sudo cat /var/log/nginx/error.log`

## File permissions

Even if you think your folders have the correct permissions, double check again. This is usually the biggest culprit. Run the following commands to apply the correct permissions.

```shell
sudo chown -R www-data:www-data /etc/nginx/
sudo chown -R www-data:www-data /var/www/
sudo chmod -R 774 /etc/nginx/
sudo chmod -R 774 /var/www/
chmod g+rwxs /etc/nginx/
chmod g+rwxs /var/www
```

This will apply the owner and group to the correct two directories, and then apply permissions so that these owners and groups have full control over the folders. Anyone else just has read access. Finally it sets it so that all new folders and files are applied the owners and group and their permissions.
