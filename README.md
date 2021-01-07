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


```

> N.B: 
>  - You should have your action set to `self-hosted` for its *runs-on* value which can be located under *build* under *jobs*
>  - To kkep the runner alive at all times, run the following in the repo directory **on the server**: 
>     - `sudo ./svc.sh install` -> To install the service as root 
>     - `sudo ./svc.sh start`  -> To start it and keep it alive even after restarts of the server


> Issues:
> - If it says *Must not run with sudo*: 
>   - **FIX**: run the following command: `export AGENT_ALLOW_RUNASROOT="1"` or *Create a new User Account* since running as root is not ideal.