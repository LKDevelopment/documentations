## Install Zabbix-Agent
on Ubuntu 18.04

### Installation
```
wget https://repo.zabbix.com/zabbix/3.5/ubuntu/pool/main/z/zabbix-release/zabbix-release_3.5-1%2Bbionic_all.deb
sudo dpkg -i zabbix-release_3.5-1+bionic_all.deb
sudo apt install zabbix-agent
# Generate random PSK 
sudo sh -c "openssl rand -hex 32 > /etc/zabbix/zabbix_agentd.psk"
# Get and save the random PSK
cat /etc/zabbix/zabbix_agentd.psk


```
### Configuration
``sudo nano /etc/zabbix/zabbix_agentd.conf``

Search for the "Server" entry and change it to the Zabbix Host

Search for the "TLSConnect" and change it to "TLSConnect=psk" (uncomment!)

Search for the "TLSAccept" and change it to "TLSAccept=psk" (uncomment!)

Search for "TLSPSKIdentity" and change it to a unique ID such as "PSK 001"

Search for "TLSPSKFile" and change it to the location to the file (``/etc/zabbix/zabbix_agentd.psk``)


### Start the Service
```
sudo systemctl start zabbix-agent
sudo systemctl enable zabbix-agent

```

### Firewall
```
sudo ufw allow 10050/tcp  
```

Now head over to the Zabbix Webinterface.

"Configuration" --> "Hosts" --> "Create Host"

Change Hostname, Groups, IP

"Templates" --> Add "Linux Servers"

"Encryption" --> Connections to host & Connections from host to PSK (nothing else!)

Set the PSK value to the generated from above.

Save