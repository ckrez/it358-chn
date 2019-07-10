Finishing up
-------------
Thank you for participating today!

## Additional Exercises
Try deploying some of the other honeypots available via the deploy scripts, or modifying the deployment scripts to do
 something different, or explore the CHN Server API.
 
## Modify a deployment script

Since these VM's run SSH on an alternate port (4222), you can have a cowrie honeypot listen on the default SSH port. 
Modify the Deployment script to change which port Cowrie runs on when deployed.

To do this, selec the `Ubuntu - Cowrie` script from the Deployment tab in CHN Server. Look at the Section entitled 
"Script". Note the lines where the docker-compose.yml file is created, specifically under the "ports:" section:

```yaml
version: '2'
services:
  cowrie:
    image: stingar/cowrie:1.8
    volumes:
      - ./cowrie.sysconfig:/etc/default/cowrie
      - ./cowrie:/etc/cowrie
    ports:
      - "2222:2222"
      - "23:2223"
```

Change the first number for the `2222:2222` entry to instead read `22:2222`. Then scroll to the bottom of the page 
and press the "Update" button to save your changes.

This change tells Docker to map the port `22` to the internal Docker network port `2222`, which is where the Cowrie 
container is listening. 

Now that the deployment script has been changed, copy the "Deploy Command" and SSH to your honeypot server. Run the 
Deploy Command on the honeypot server, then use `docker ps` to verify that the honeypot is now listening on the 
default SSH port.

## Deploy a Dionaea Honeypot

Open a new terminal tab/window, and export your team number again; for example if your team number is 22:
```bash
export TEAM=22
```

Prepare your honeypot vm for deploying a new honeypot. First, let's stop the existing honeypot and remove it's 
information:

```bash
ssh -l ubuntu -p 4222 workshop-chn-hp-${TEAM}.security.duke.edu
cd ~
docker-compose down
docker system prune
sudo rm -rf ~/*
```

Now go back to the CHN Server web interface, navigate to the Deploy tab, select the `Ubuntu - Dionaea` script, and 
copy the command. Then paste this command into the honeypot terminal window. This should take a moment, mostly in 
downloading the new Docker image.

Once this is complete, go back to the web interface for CHN Server and check the Sensors tab to see the new sensor.

Explore what services Dionaea provides and see if you can create an event. Check your logs and CIF to see if that 
event shows up there as well.

Hint: `docker-compose ps` on the honeypot server can help you identify which ports are open.

## Explore the CHN Server API

Please refer to the [project documentation](https://communityhoneynetwork.readthedocs.io/en/latest/serverapi/) for 
details on the API.
