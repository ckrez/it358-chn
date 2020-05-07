## Exploring your CIF data

## Installing CIF Tools
Seeing that data is submitted via logs is handy, but not very useful for using the data. We'll install the CIF SDK, 
which provides some command line tools for selecting, retrieving, and formatting data from a CIF server.

For this workshop, we’ll install the CIFv3 tools onto the CHN Server system, but the SDK can be installed on any 
Linux system. 

Begin by SSHing into the CHN Server host:

On Mac/Linux:
```bash
ssh -l ubuntu -p 4222 ${GROUP CHN SERVER}
```
On Windows:
```bash
ssh -l ubuntu -p 4222 ${GROUP CHN SERVER} 
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
remote: ${CIF SERVER URL}
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
 sure to substitute your group number in the provider field. For instance, if you're group number was 50:
 
**NOTE: All indicators should be treated as malicious. Handle with care (i.e. don't click / visit URLs)**
 
```bash
cif --tags honeypot --itype ipv4 --provider group-50
```

Trying again without the `--provider` option will show you any other submissions from your classmates!

```bash
cif --tags honeypot --itype ipv4
```

### Generating a blocking feed

You can generate a feed that could be loaded into a firewall for blocking purposes. Generate a feed and save
it to a file:


```bash
cif --tags honeypot --itype ipv4 --feed --format csv --fields indicator > group-50_feed.txt
```

Run the `wc` command to see how large your feed would be:

```bash
wc -l group-50_feed.txt
```

### Looking at other interesting data

Run the following commands to see what results you get:

```bash
cif --tags honeypot --itype fqdn
cif --tags honeypot --itype url
```

**Questions**:
- What kind of indicators (IPs, Domains, URLs, etc.) were returned?
- What do you think these indicators represent? 