# Github ELK Setup

# 教學影片
https://www.youtube.com/watch?v=TfhcJXdNSdI&t=358s&ab_channel=EvermightTech

# Domain name

| ip | Host |
| -------- | -------- |
| 192.168.100.211     | node1     | 
| 192.168.100.212     | node2     | 
| 192.168.100.213     | node3     | 
| 192.168.100.214     | node4     | 
| 192.168.100.215     | node5     | 
| 192.168.100.216     | node6     |
| 192.168.100.217     | node7     | 
| 192.168.100.218     | kibana     | 

# Install elastic 8.X.X(Current version)

install.sh's contents
```
#!/bin/bash

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg

sudo apt-get install apt-transport-https

echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list

sudo apt-get update && sudo apt-get install elasticsearch

```
**After you finish install** you will get
```
### NOT starting on installation, please execute the following statements to configure elasticsearch service to start automatically using systemd
 sudo systemctl daemon-reload
 sudo systemctl enable elasticsearch.service
### You can start elasticsearch service by executing
 sudo systemctl start elasticsearch.service
```
### Reset password

# Add node to cluster

```root@node1:/usr/share/elasticsearch/bin# ./elasticsearch-create-enrollment-token -s node```

## Token

<font color="#f00">You will receive Token
</font>


## Node2~Node7


Example : Node2
```
root@node2:/usr/share/elasticsearch/bin# ./elasticsearch-reconfigure-node --enrollment-token eyJ2ZXIiOiI4LjExLjEiLCJhZHIiOlsiMTkyLjE2OC4xMDAuMjExOjkyMDAiXSwiZmdyIjoiOGI1MGYxNTZhZGQ5ODFmYjg2MDY1YjgyOTc2YmFiYjMxNTE3NDY2OTQxM2Y3OGE3Y2Y4MDIzZDIzOTQ4NmZlOSIsImtleSI6IkVoZS1FNHdCNzNLbnVRUzI2MEVWOjBiTHF2c1g1VEdPdDA3bktpUHRiNlEifQ==
```
### SSL certificate Example

![image](https://github.com/Kenny890806/Elasticsearch-Cluster-Kibana/blob/main/image/SSL_3.png)
![image](https://github.com/Kenny890806/Elasticsearch-Cluster-Kibana/blob/main/image/ELK_yml.png)


key: /path/to/file/nsysu.key
certificate: /path/to/file/nsysu.cer


## Summary: Adding Node to Cluster

### elasticsearch.yml
- **edit these fields**
    - cluster.name
    - node.name
    - network.host
    - network.post
    - xpack.security.http.ssl(optional)
- **review these fields**
    - discovery.seed_hosts
    - transport.hosts



# Add Kibana

# Install kibana 8.X.X(Current version)

install.sh如下
```
#!/bin/bash

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg

sudo apt-get install apt-transport-https

echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list

sudo apt-get update && sudo apt-get install kibana


```

## Kibana

## Input in Windows CMD 
```
curl -X POST -u elastic:@SwDivision https://192.168.100.211:9200/_security/service/elastic/kibana/credential/token/kibana_token
```


### Token

<font color="#f00">You will receive Token
</font>



### cd to **/usr/share/kibana/bin/**
```
$ ./kibana-keystore add elasticsearch.serviceAccountToken

Input token: ******************************
```

### SSL certificate Example

![image](https://github.com/Kenny890806/Elasticsearch-Cluster-Kibana/blob/main/image/kibana_yml.png)


server.ssl.certificate: /path/to/file/nsysu.cer
server.ssl.key: /path/to/file/nsysu.key





## Delete Elasticsearch (For Restart)
```
**sudo apt-get --purge autoremove elasticsearch**
```


