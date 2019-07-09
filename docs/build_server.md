Building CHN Server
===================

Connect to your workshop-chn-server host.

### Preparation
Prepare to install CHN Server by creating the directory for CHN Server files.

```bash
sudo apt install -y python3 python3-pip && sudo pip3 install validators && sudo git clone https://github.com/CommunityHoneyNetwork/chn-quickstart.git chnserver && sudo chown ubuntu:docker /opt/chnserver
```

### Build CHN Server

To build CHN Server, simply run the guided_docker_compose.py quickstart script:

```bash
cd /opt/chnserver
python3 guided_docker_compose.py
```

Since we're using a valid name on a publicly accessible IP address, we can use Let'sEncrypt to obtain a valid cert. 

When prompted, input the valid domain name for the CHN Server instance, and set the Certificate Strategy to "CERTBOT."

Feel free to answer "no" to the questions about logging and CIF. See below for sample output:

```text
Please enter the domain for your CHN Instance.  Note that this must be a resolvable domain.
Domain: workshop-chn-server-${TEAM}.security.duke.edu  
Please enter a Certificate Strategy.  This should be one of:

CERTBOT: Signed certificate by an ACME provider such as LetsEncrypt.  Most folks will want to use this.  You must ensure your URL is accessable from the ACME hosts for verification here
BYO: Bring Your Own.  Use this if you already have a signed cert, or if you want a real certificate without CertBot
SELFSIGNED: Generate a simple self-signed certificate
Certificate Strategy: CERTBOT
Wrote file to config/sysconfig/chnserver.sysconfig
Wrote docker-compose.yml

Do you wish to enable logging to a remote CIFv3 server? [Y/n]: n

Do you wish to enable logging to a local file? [Y/n]: n
```

## Start the server

Start the CHN Server docker containers with the "-d" flag to run the containers in the background (daemon mode).

```bash
docker-compose up -d
```

This process should take several minutes as the machine pulls all the relevant images from DockerHub. When the 
process completes and your prompt returns, you should be able to browse to `https://workshop-chn-server-TEAM.security.duke.edu` 
in a web browser and recieve a login screen to verify that CHN Server is running. Be sure to substitute your team 
number for TEAM in the URL.
