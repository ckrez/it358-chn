Setting Up Logging for Honeypot Data
====================================

## Connect to the CHN Server Host
If you do not already have an SSH session established with your workshop-chn-server, SSH into your CHN Server instance.

On Mac/Linux:
```bash
ssh -l ubuntu -p 4222 workshop-chn-server-${TEAM}.security.duke.edu 
```
On Windows:
```bash
ssh -l ubuntu -p 4222 workshop-chn-server-$env:TEAM.security.duke.edu 
```

## Create hpfeeds-logger.sysconfig

Like when starting CHN Server, simply run the following commands:

```bash
cd /opt/chnserver && python3 guided_docker_compose.py
```

Answer "n" when asked about reconfiguring CHN Server.
Answer "n" when asked about logging to a remote CIFv3 server.
Answer "y" when asked about logging to a local file.
Fill in "splunk" for the Logging Format

```text
Previous chn-server.sysconfig file detected. Do you wish to reconfigure? [y/N]: n
Do you wish to enable logging to a remote CIFv3 server? [y/N]: n
Do you wish to enable logging to a local file? [y/N]: y

splunk: Comma delimited key/value logging format for use with Splunk
json: JSON formatted log format
arcsight: Log format for use with ArcSight SIEM appliances
json_raw: Raw JSON output from hpfeeds. More verbose that other formats, but also not normalized. Can generate a large amount of data.
Logging Format: splunk
Wrote file to config/sysconfig/hpfeeds-logger.sysconfig
```

## Start the hpfeeds-logger container

Start the hpfeeds-logger container with the following command:

```bash
docker-compose up -d hpfeeds-logger
```

## Checking your setup
You should now have a new directory `/opt/chnserver/storage/hpfeeds-logs`. List this directory to see a new file, chn-splunk
.log:

```bash
ls -l /opt/chnserver/storage/hpfeeds-logs
```

You can show the contents of this file with the `cat` command:

```bash
cat /opt/chnserver/storage/hpfeeds-logs/chn-splunk.log
```
Your file may be empty at the moment. To test your honeypot, we can simulate an attack by trying to log into our new 
honeypot using SSH to port 2222. 

## Test your honeypot
Open a new terminal tab/window, and export your team number again; for example if your team number is 50:
```bash
export TEAM=50
```

Now, attempt to connect to the honeypot SSH port:

On Mac/Linux:
```bash
ssh -l ubuntu -p 2222 workshop-chn-hp-${TEAM}.security.duke.edu 
```
On Windows:
```bash
ssh -l ubuntu -p 2222 workshop-chn-hp-$env:TEAM.security.duke.edu 
```

If you're using Putty, you can use the "Load" button for your normal workshop-chn-hp connection, and change the port 
before connecting to simulate an attack.

You can now `cat /opt/chnserver/storage/hpfeeds-logs/chn-splunk.log` the log file again, and you should see data about your 
attempt to log in.
