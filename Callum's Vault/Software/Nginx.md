
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
## Docker 

### 1. Install Docker

Please refer to the Docker installation section to see how to install Docker: [[Docker#Installation]]

### 2. Create Docker compose file

Create a file called `docker-compose.yaml` and paste the following into it, editing as required:

```yaml
services:
    client:
        image: nginx
        ports:
            - 80:80
            - 443:443
        volumes:
            - /var/www/:/var/www/
```

### 3. Run Docker compose

```Shell
docker compose up
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

## Create folders for the subdomains

Create your folders in the same location that hosts your main domain `/var/www/sub.domain_name.co.uk`

For example, it should look like this:

- /var/www/callumwellard.co.uk - main domain of callumwellard.co.uk
- /var/www/jellyfin.callumwellard.co.uk - Subdomain for Jellyfin
- /var/www/webapp.callumwellard.co.uk - Subdomain for a webapp

## Correct ownership of the subdomains

Ensure that Nginx can access these folders by changing ownership of the folders to `www-data`; the account used by Nginx.

```Shell
sudo chown -R www-data:www-data /var/www/sub.domain_name.co.uk
```

With the example above, it should look like so:

```Shell
sudo chown -R www-data:www-data /var/www/jellyfin.callumwellard.co.uk
sudo chown -R www-data:www-data /var/www/webapp.callumwellard.co.uk
```

## Make the subdomains available

Create the configuration files for your subdomains in the following location `/etc/nginx/sites-available/` with the name of the subdomain.

```Shell
sudo nano /etc/nginx/sites-available/sub.domain_name.co.uk
```

Inside the file, paste the following and make sure to replace `sub.domain_name.co.uk` with your own subdomain:

>[!Important]
> You do not need to paste the following if you are not using the sub domain as a webhost

```Text
server {
        listen 80;
        listen [::]:80;

        root /var/www/sub.domain_name.co.uk/html;
        index index.html index.htm index.nginx-debian.html;

        server_name sub.domain_name.co.uk www.sub.domain_name.co.uk;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

## Enable the domain

Enable the site and make it live by creating a symbolic link in `/etc/nginx/sites-enabled`:

```Shell
sudo ln -s /etc/nginx/sites-available/sub.domain_name.co.uk /etc/nginx/sites-enabled/
```

## Create a config file (Redirect traffic to a different host)

>[!Important]
> If you are using the subdomain as a redirect, follow these steps. Otherwise ignore them.

Now create a `conf` file so that you can tell Nginx what to do with the request. Use the subdomain name as the name of the file to make it easier to track.

```Shell
sudo nano /etc/nginx/conf.d/subdomain.conf
```

Inside that file, paste the following:

```conf
server {
        listen 80;
        listen [::]:80;

		server_name sub.domain_name.co.uk www.sub.domain_name.co.uk;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

### Passing the request to the correct server

Now that you have enabled the site, you can tell Nginx where to forward the requests it gets for your certain subdomain. **Inside the same file**, paste the following inside the `server` block:

```conf
location / {
        # Proxy main traffic
        proxy_pass http://IP.GOES.HERE:PORT;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Protocol $scheme;
        proxy_set_header X-Forwarded-Host $http_host;
    }
```

> [!Warning]
> Be sure to change the IP address and port number.

## Check syntax errors

After restarting the service and Nginx has successfully applied the changes, check that the config files have successfully applied and have no errors by using the following command:

```Shell
sudo nginx -t
```

## Restart the Nginx service

Restart the service by entering the following command. This should be ran anytime ***Nginx*** change has been made:

```Shell
sudo systemctl restart nginx
```

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

A message will appear asking how you would like to authenticate. Select the option with `Nginx Web Server plugin (nginx)`

## Edit your server blocks

### HTML page

With Certbot now active, edit your Server blocks in the `/etc/nginx/sites-available` directory to enable SSL.

If using a `site-enabled`, use the following:

```
server {
	listen 80;
	listen [::]:80;

	root /var/www/sub.domain_name.co.uk/html;
	index index.html index.htm index.nginx-debian.html;

	server_name sub.domain_name.co.uk www.sub.domain_name.co.uk;

	# location / {
		# try_files $uri $uri/ =404;
	# }
}

server {
	# Binds the TCP port 443 and enable SSL.
    listen 443 ssl;
	listen [::]:443;
	# Root directory used to search for a file        
	root /var/www/sub.domain_name.co.uk/html; 
	# Defines the domain or subdomain name. 
    # If no server_name is defined in a server block then 
	# Nginx uses the 'empty' name
    server_name domain_name.co.uk www.domain_name.co.uk;

	# Path of the SSL certificate
    ssl_certificate /etc/letsencrypt/live/sub/domain_name.co.uk/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/sub.domain_name.co.uk/privkey.pem;
	# Use the file generated by certbot command.
    include /etc/letsencrypt/options-ssl-nginx.conf;
	# Define the path of the dhparam.pem file.
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

	location / {
		# Return a 404 error for instances when the server receives 
	    # requests for untraceable files and directories.
        try_files $uri $uri/ =404;
	}

}
```

### Redirecting traffic

If using `.conf`, edit the following in `/etc/nginx/conf.d`:

> [!Tip]
> Note that now you can redirect the HTTP requests straight to HTTPS

```conf
server {
		# Redirect to HTTPS
        listen 80;
        listen [::]:80;
        server_name sub.domain.co.uk www.sub.domain.co.uk;
        return 301 https://sub.domain.co.uk;
}

server {
        listen 443 ssl;
        listen [::]:443;

        server_name sub.domain.co.uk www.sub.domain.co.uk;

        # Path of the SSL certificate
        ssl_certificate /etc/letsencrypt/live/sonarr.callumwellard.co.uk/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/sonarr.callumwellard.co.uk/privkey.pem;
        # Use the file generated by certbot command.
        include /etc/letsencrypt/options-ssl-nginx.conf;
        # Define the path of the dhparam.pem file.
        ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

        location / {
                # Proxy main traffic
                proxy_pass http://192.168.0.68:8989;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
                proxy_set_header X-Forwarded-Protocol $scheme;
                proxy_set_header X-Forwarded-Host $http_host;
        }
}
```
## Restart the Nginx service

Restart the service by entering the following command. This should be ran anytime ***Nginx*** change has been made:

```Shell
sudo systemctl restart nginx
```