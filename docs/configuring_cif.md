Set Up Logging to CIFv3
=======================

## Configuring logging to CIFv3

If you do not already have an SSH session established with your workshop-chn-server, SSH into your CHN Server instance.

On Mac/Linux:
```bash
ssh -l ubuntu -p 4222 workshop-chn-server-${TEAM}.security.duke.edu 
```
On Windows:
```bash
ssh -l ubuntu -p 4222 workshop-chn-server-$env:TEAM.security.duke.edu 
```

## Updating CHN Server to log to CIFv3

Like when starting CHN Server, simply run the following commands:

```bash
cd /opt/chnserver && python3 guided_docker_compose.py
```

Answer "n" when asked about reconfiguring CHN Server.

When you reach the question about enabling CIFv3, answer 'Y'.

Enter the provided information for the CIF host URL, including the "https" portion:

```text
https://cif-server.security.duke.edu
```

Enter the provided information for the API Token:

```text
0a0d81f023a6e5c16c7467a0b14c5a5da4e16f08dd51c0d9c9bcbbdc57f3dd273ade35606245838a
```

Also be sure to update the CIF_PROVIDER field to "team-NUMBER", where `NUMBER` is the team number you were assigned.

```text
Previous chn-server.sysconfig file detected. Do you wish to reconfigure? [Y/n]: n
Do you wish to enable logging to a remote CIFv3 server? [Y/n]: Y
Please enter the URL for the remote CIFv3 server: https://cif-server.security.duke.edu
Please enter the API token for the remote CIFv3 server: 0a0d81f023a6e5c16c7467a0b14c5a5da4e16f08dd51c0d9c9bcbbdc57f3dd273ade35606245838a
Please enter a name you wish to be associated with your organization: team-${TEAM}
Wrote file to config/sysconfig/hpfeeds-cif.sysconfig
Previous hpfeeds-logger.sysconfig file detected. Do you wish to reconfigure? [y/N]: n
```

## Start the hpfeeds-cif container

Start hpfeeds-cif container with the following command:

```bash
docker-compose up -d hpfeeds-cif
```

## Test your CIF install

Please generate another honeypot event to ensure there's data to find.

Open a new terminal tab/window, and export your team number again; for example if your team number is 50:

On Mac/Linux:
```bash
export TEAM=50
```
On Windows:
```bash
$env:TEAM=50
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

Now examine the hpfeeds-cif logs to see if you see a new event:

```bash
docker-compose logs --tail=10 hpfeeds-cif
```

## Installing CIF Tools
Seeing that data is submitted via logs is handy, but not very useful for using the data. We'll install the CIF SDK, 
which provides some command line tools for selecting, retrieving, and formatting data from a CIF server.

For this workshop, we’ll install the CIFv3 tools onto the CHN Server system, but the SDK can be installed on any 
Linux system. 

Begin by SSHing into the CHN Server host:

On Mac/Linux:
```bash
ssh -l ubuntu -p 4222 workshop-chn-server-${TEAM}.security.duke.edu 
```
On Windows:
```bash
ssh -l ubuntu -p 4222 workshop-chn-server-$env:TEAM.security.duke.edu 
```

Once connected, install the CIFv3 pip package:

```bash 
sudo apt install python-pip -y && sudo pip install 'cifsdk>=3.0.0,<4.0'
```

Next you need to create a new file, `/home/ubuntu/.cif.yml`, with the following contents. Remember to use either nano
 or vim to create this file:

Create the file:
```bash
nano /home/ubuntu/.cif.yml
```

Then paste in these contents:
```yaml
token: <same value as CIF_TOKEN>
remote: https://cif-server.security.duke.edu
no_verify_ssl: true
```
Now hit `Ctrl-X` to quit, answer `y` to save, and hit enter to keep your filename.

You can now verify that you can connect to the CIF instance with the SDK using the following command:

```bash
cif -p
```
Which, when correctly configured and working properly, will return results like:
```bash
roundtrip: 0.115531921387 ms
roundtrip: 0.0400021076202 ms
roundtrip: 0.0375020503998 ms
roundtrip: 0.0692510604858 ms
```

## Querying CIF
Now you can search for the entry you generated earlier, looking for your provider name with the following command. Be
 sure to substitute you team number in the provider field. For instance, if you're team number was 50:
 
```bash
cif --tags honeypot --itype ipv4 --last-hour --provider team-50 
```

Trying again without the `--provider` option will show you any other submissions from your classmates!

```bash
cif --tags honeypot --itype ipv4 --last-hour
```
## Bonus

Check out the [project documentation](https://communityhoneynetwork.readthedocs.io/en/latest/hpfeeds-cif/#selecting-and-formatting-data) 
for some more ideas on how you can use the CIF client to select and format data.



