Setting Up Logging for Honeypot Data
====================================

## Connect to the CHN Server Host
SSH into your CHN Server instance and cd to to the "chnserver" directory.

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
cd /opt/chnserver
python3 guided_docker_compose.py
```

Answer "n" when asked about reconfiguring CHN Server and hpfeeds-cif.

Feel free to fill in "splunk" for the Logging Format

```text
Previous chn-server.sysconfig file detected. Do you wish to reconfigure? [Y/n]: n
Previous hpfeeds-cif.sysconfig file detected. Do you wish to reconfigure? [Y/n]: n
Do you wish to enable logging to a local file? [Y/n]: Y
Please enter a Certificate Strategy.  This should be one of:

splunk: Comma delimited key/value logging format for use with Splunk
json: JSON formatted log format
arcsight: Log format for use with ArcSight SIEM appliances
json_raw: ('Raw JSON output from hpfeeds. More verbose that other formats,', 'but also not normalized. Can generate a large amount of data.')
Logging Format: splunk
Wrote file to config/sysconfig/hpfeeds-logger.sysconfig
Updated docker-compose.yml
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
ssh -l ubuntu -p 2222 workshop-hp-server-${TEAM}.security.duke.edu 
```
On Windows:
```bash
ssh -l ubuntu -p 2222 workshop-hp-server-$env:TEAM.security.duke.edu 
```

If you're using Putty, you can use the "Load" button for your normal workshop-hp-server connection, and change the port 
before connecting to simulate an attack.

You can now `cat /opt/chnserver/storage/hpfeeds-logs/chn-splunk.log` the log file again, and you should see data about your 
attempt to log in.
