<h1>SIEM with Wazuh, TheHive and SOAR through Shuffle</h1>


<h2>Description</h2>
This project consists of setting up a SIEM through cloud services and VMs. Wazuh and TheHive will assist us in case managment, while Shuffle will be our SOAR platform.

<br>
<br />

> [!NOTE]
>Please refer to the Installation Instructions file before attempting this.




<h2>Utilities Used</h2>

- <b>Wazuh</b>
- <b>TheHive</b>
- <b>Shuffle</b>
- <b>Virtualbox</b>

<h2>Environments Used</h2>

- <b>Azure/Digital Ocean</b>
- <b>Windows 10</b> (21H2)
- <b>Ubuntu 22.04 LTS</b>

## Program walk-through

### Configuring Cassandra:

The main things we want to configure are the "listen_address", "rpc_address" and the "seeds" in the "cassandra.yaml" file. Change the Localhost in all of them to your TheHive public IP address.
```
nano /etc/cassandra/cassandra.yaml
```
<img src="https://i.imgur.com/0Xz6tuN.png" height="80%" width="80%"/>

Afterwards we have to stop cassandra, remove the old files and start it again:
```
systemctl stop cassandra.service
rm -rf /var/lib/cassandra/*
systemctl start cassandra.service
```
<h2> </h2>

### Configuring ElasticSearch:

First, in the "elasticsearch.yml" file change the cluster name to whatever you like. Then add your TheHive IP address in "network.host:". Finally, uncomment the "http.port:", "cluster.initial_master_nodes:" and remove node-2.
```
nano /etc/elasticsearch/elasticsearch.yml
```
<img src="https://i.imgur.com/JXoOF9k.png" height="80%" width="80%"/>

Now we have to start and enable ElasticSearch:
```
systemctl start elasticsearch
systemctl enable elasticsearch
```
<h2> </h2>

### Configuring TheHive:
We start by changing the TheHive ownership to the destination directories:
```
chown -R thehive:thehive /opt/thp
```
Next, in the "application.conf" file change all the "hostname" and "application.baseUrl" to your TheHive IP address:

```
nano /etc/thehive/application.conf
```

<img src="https://i.imgur.com/BJjQZWb.png" height="80%" width="80%"/>

Once again, we have to start and enable TheHive:
```
systemctl start TheHive
systemctl enable TheHive
```

<h2> </h2> 

### Adding The Agent

In the Wazuh directory, there's a tar file containing the credentials we'll need to log in.  The main two are "admin user" and "API user"

```
tar -xvf wazuh-install-files.tar
cd wazuh-install-files
cat wazuh-passwords.txt 
```
Now we need to add an agent to start collecting telemetry. In the Wazuh dashboard click on "Add agent" and follow the prompts. Be sure to set your Wazuh public IP address as the server IP address.
<br/>
> [!TIP]
> You'll need to run the terminal with admin privileges when adding the agent


#### Configuring The Agent

To ingest logs for our telemtry collection, we need to edit the "ossec.conf" file. You'll either find it in "C:\Program Files (x86)\ossec-agent" for Windows OS or in "/var/ossec/etc/" for Linux OS.
<br/>
Inside the file you'll find "<! -- Log Analysis -->", this is where you can add the location of the logs you want to collect. Make sure to restart Wazuh after editing the conf file.

<img src="https://i.imgur.com/GPz81qk.png" height="80%" width="80%"/>
<br/>

> [!IMPORTANT]
> It's best practice to create a backup file in case things go wrong!


<h2> </h2> 



Wait for process to complete (may take some time):  <br/>
<img src="https://i.imgur.com/JL945Ga.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
