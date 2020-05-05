Prepare your hosts
===================
## Connecting to your CHN hosts


We'll begin the server install by SSHing to the CHN Server host and Honeypot host on port 4222, using the username 'ubuntu' username and 
SSH private key. 

On Mac/Linux:
```bash
ssh -l ubuntu -p 4222 -i it358.pem ${GROUP CHN SERVER} 
ssh -l ubuntu -p 4222 -i it358.pem ${GROUP HP SERVER} 
```
On Windows:
```bash
ssh -l ubuntu -p 4222 -i it358.pem ${GROUP CHN SERVER} 
ssh -l ubuntu -p 4222 -i it358.pem ${GROUP HP SERVER} 
```
 
## Preparing the hosts

First we'll update the repositories on the host and install docker using apt. Later we'll need to reconnect, so let's 
also upgrade all packages. After the package is installed, we'll ensure our ubuntu user is in the docker group, and 
that docker is set to start on boot. Finally, we'll reboot the system to ensure a clean starting point.

Paste the following line into the terminal window for both the CHN Server host as well as the honeypot host. 

```bash
sudo apt update && sudo apt upgrade -y && sudo apt install -y docker-compose jq && sudo usermod -aG docker ubuntu && sudo systemctl enable docker && sudo reboot
```

Once your host reboots, your session will end an you will need to wait approximately 30 seconds and then reconnect to
 the CHN Server host and honeypot host.

On Mac/Linux:
```bash
ssh -l ubuntu -p 4222 ${GROUP CHN SERVER}  
```
On Windows:
```bash
ssh -l ubuntu -p 4222 ${GROUP CHN SERVER} 
```

and

On Mac/Linux:
```bash
ssh -l ubuntu -p 4222 ${GROUP HP SERVER} 
```
On Windows:
```bash
ssh -l ubuntu -p 4222 ${GROUP HP SERVER} 
```

*A Note on Editors*
In the terminal environment you will need to add text to files. There are two editors available by default on the 
hosts we've provided: `nano` and `vim`. If you don't know which you'd prefer, please use `nano`. To edit or create a 
new file, simply type:

```bash
nano filename
```



