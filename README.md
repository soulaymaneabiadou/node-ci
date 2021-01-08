# labs
Just a dummy repo for learning Git &amp; GitHub

## Actions

### Runner
> On my server do the following,
> This can be found in the settings tab of the project repo on GitHub,
> It is assuming that the srever is running a flavor of Linux x64

#### Download
```bash
# Create a folder
mkdir actions-runner && cd actions-runner

# Download the latest runner package
curl -O -L https://github.com/actions/runner/releases/download/v2.275.1/actions-runner-linux-x64-2.275.1.tar.gz

# Extract the installer
tar xzf ./actions-runner-linux-x64-2.275.1.tar.gz
```

#### Configure
```bash
# Create the runner and start the configuration experience
./config.sh --url https://github.com/soulaymaneabiadou/labs --token AK7TGBOM6H6W4WQS3ZQRVSS766IPS

# Last step, run it!
./run.sh

# Install & run the service
sudo ./svc.sh install
sudo ./svc.sh start
```

### Setup NGINX
```bash
# Install it
sudo apt install nginx

# Check status
sudo systemctl status nginx

# Start it (If not running)
sudo systemctl start nginx

# Setup a Config
cd /etc/nginx/sites-available/

# Restart nginx
sudo systemctl restart nginx

# go back to the `actions-runner/` directory, then to the 'app/' folder inside of it and then to the repo folder inside that and make nginx to point to this

# Edit the 'default' file
sudo nano default # and then go to line that says `root /var/www/build` and put the repo's directory path and restart nginx again
```

### Seting Up an Express app
> First steps are the same as before,
> In terms of creating and configuring the runner in the repo,
> Add `PM2`

#### Adding PM2
```
sudo npm i -g pm2

pm2 start file --name=NameHere
```

#### Make the runner restart the PM2 server
> In the runner file, do the following: at the end add: 
> `- run: pm2 restart NameHere`

### Setup Proxy with NGINX
```bash
# start the ufw
sudo ufw enable

sudo ufw allow ssh

sudo ufw allow 'Nginx Full'

# Use nginx to forward the requests
# Edit the nginx default file, add a location block after the existing one
# This is assuming that the API endpoints are prefixed with /api
location /api {
  rewrite ^\/api\/(.*)$ /api/$1 break;
  proxy_pass http://localhost:5000;
}
```

### Setup a Server Block
```bash
# Make a copy of the nginx default config
sudo cp default namehere.com

# Edit namehere.com, change the `server_name` to point to our domain name, like
nano namehere.com

# Remove the default_server on the top two rows under the server {
# And put this in front of the server_name, and then save it
namehere.com www.namehere.com

# Create a sim link & Restart the nginx
sudo ln -s /etc/nginx/sites-available/namehere.com /etc/nginx/sites-availabled
```

### Secure the site, Add SSL Certificate
```bash
# Install certbot
sudo add-apt-repository ppa:certbot/certbot

sudo apt install python-certbot-nginx

sudo apt install python3-certbot-nginx

sudo certbot --nginx -d namehere.com -d www.namehere.com
# And follow the steps to make it redirect http to https and then you are done
```

> N.B: 
>  - You should have your action set to `self-hosted` for its *runs-on* value which can be located under *build* under *jobs*
>  - To kkep the runner alive at all times, run the following in the repo directory **on the server**: 
>     - `sudo ./svc.sh install` -> To install the service as root 
>     - `sudo ./svc.sh start`  -> To start it and keep it alive even after restarts of the server


> Issues:
> - If it says *Must not run with sudo*: 
>   - **FIX**: run the following command: `export AGENT_ALLOW_RUNASROOT="1"` or *Create a new User Account* since running as root is not ideal.