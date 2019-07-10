Prepare your hosts
===================
## Connecting to your CHN hosts and change passwords
Ensure you know the team number for the servers you will be using. We'll then export a variable to your local 
terminal instance to make copy/pasting these commands easier. So for instance, if your team number is 50, you will run:

On Mac/Linux:
```bash
export TEAM=50
```
On Windows:
```bash
$env:TEAM=50
```
This will ensure that each of the commands below gets the proper team number substituted in the proper place.

We'll begin the server install by SSHing to the CHN Server host on port 4222, using the username 'ubuntu' username and 
password

On Mac/Linux:
```bash
ssh -l ubuntu -p 4222 workshop-chn-server-${TEAM}.security.duke.edu 
```
On Windows:
```bash
ssh -l ubuntu -p 4222 workshop-chn-server-$env:TEAM.security.duke.edu 
```

Now, first things first: **Change the password for your user!** Please try to make it something that can withstand the 
general internet, as these hosts are publicly exposed. 

To change your password issue this command:

```bash
passwd
```
You will be prompted for your current password, and then to enter your new password twice. 

This step is important because in a group this size, there's always at least one person who thinks it's funny to mess
 with someone elses' machine.
 
**Please repeat this process with your honeypot server as well.**

Open a new terminal/session and log into the honeypot host; note that this hostname uses `-hp-` rather than `-chn-` in 
the hostname.

On Mac/Linux:
```bash
ssh -l ubuntu -p 4222 workshop-chn-hp-${TEAM}.security.duke.edu 
```
On Windows:
```bash
ssh -l ubuntu -p 4222 workshop-chn-hp-$env:TEAM.security.duke.edu 
```

To change your password issue this command:

```bash
passwd
```
You will be prompted for your current password, and then to enter your new password twice. 
 
## Preparing the hosts

First we'll update the repositories on the host and install docker using apt. Later we'll need to reconnect, so let's 
also upgrade all packages. After the package is installed, we'll ensure our ubuntu user is in the docker group, and 
that docker is set to start on boot. Finally, we'll reboot the system to ensure a clean starting point.

Paste the following line into the terminal window for both the CHN Server host as well as the honeypot host. You will
 be prompted to provide your password as we're running the privileged command `sudo`, which means "run as super-user".

```bash
sudo apt update && sudo apt upgrade -y && sudo apt install -y docker-compose jq && sudo usermod -aG docker ubuntu && sudo systemctl enable docker && sudo reboot
```

Once your host reboots, your session will end an you will need to wait approximately 30 seconds and then reconnect to
 the CHN Server host and honeypot host.

On Mac/Linux:
```bash
ssh -l ubuntu -p 4222 workshop-chn-server-${TEAM}.security.duke.edu 
```
On Windows:
```bash
ssh -l ubuntu -p 4222 workshop-chn-server-$env:TEAM.security.duke.edu 
```

and

On Mac/Linux:
```bash
ssh -l ubuntu -p 4222 workshop-chn-hp-${TEAM}.security.duke.edu 
```
On Windows:
```bash
ssh -l ubuntu -p 4222 workshop-chn-hp-$env:TEAM.security.duke.edu 
```

*A Note on Editors*
In the terminal environment you will need to add text to files. There are two editors available by default on the 
hosts we've provided: `nano` and `vim`. If you don't know which you'd prefer, please use `nano`. To edit or create a 
new file, simply type:

```bash
nano filename
```



