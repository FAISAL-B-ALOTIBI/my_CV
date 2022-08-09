#ELK vs cloudwatch

Cloudwatch can be used as a centralised logging service for smaller and less complex applications but if your environment is complex and big then you will be left with no option than going for a third party option like Loggly, New Relic or ELK.

Lets take example of ELK. Using logstash you can enrich your logs with extra transformation out of the box. Elasticsearch will give you wide range of search options. And Kibana will provide you a wide range of dashboards. In cloudwatch if you want the same then you can use lambda for log transformation and from there you can load it aws Elasticsearch for added search functionality.

In our above scenario only using cloudwatch does not stand a chance against ELK. But combine it with lambda, kinesis and Elasticsearch and you start to see some competition. So decide accordingly.


#install elasticsearch

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
echo "deb https://artifacts.elastic.co/packages/oss-7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
sudo apt-get update && sudo apt-get install elasticsearch

Open the Elasticsearch configuration file at: `*/etc/elasticsearch/elasticsearch.yml*` and apply the following configurations:

network.host: "localhost"
http.port: 9200
cluster.initial_master_nodes: ["<**PrivateIP**>"]

Start the Elasticsearch service:

```
sudo service elasticsearch stop
sudo service elasticsearch start
sudo service elasticsearch restart
service elasticsearch status
```

Verify the installation by cURLing:

```
sudo curl http://localhost:9200
```



**To install Kibana, use this command:**

```
sudo apt-get install kibana
```

Open the Kibana configuration file and enter the following configurations:

```
sudo vim /etc/kibana/kibana.yml
```

```
server.port: 5601
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://localhost:9200"]
```

**Start Kibana:**

```
sudo service kibana stop
sudo service kibana start
sudo service kibana restart
systemctl status kibana -l
service kibana status
/usr/share/kibana/bin/kibana --version
```

**Install Logstash**

```yaml
sudo apt-get install logstash
sudo service logstash stop
sudo service logstash start
service logstash status
systemctl start logstash -l
/usr/share/logstash/bin/logstash --version
```