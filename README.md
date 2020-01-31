## Usage
Simply execute `vagrant up` in project directory.

## Network Information
This mockup environment uses 3 ubuntu/bionic64 virtual machines.
- slave:
    IP: `10.0.0.10`
    Runs:
    - Filebeat:
        Sends syslog to `10.0.0.11:9004 Logstash@master` (defined in ./ansible/roles/filebeat_slave/files/filebeat.yml)
    - Metricbeat:
        Sends metric data to `10.0.0.11:9004 Logstash@master` (defined in ./ansible/roles/metribeat/files/metricbeat.yml)
        Sets Kibana dashboards up
- master:
    IP: `10.0.0.11`
    Runs:
    - Filebeat:
        Sends syslog to `10.0.0.11:9004 Logstash@master` (defined in ./ansible/roles/filebeat_master/files/filebeat.yml)
    - Metricbeat:
        Sends metric data to `10.0.0.11:9004 Logstash@master` (defined in ./ansible/roles/metribeat/files/metricbeat.yml)
        Sets Kibana dashboards up
    - Logstash:
        Listens port `9004`
        Sends filtered logs and plain metrics to `11.0.0.10:9200 elasticsearch@cloud` (defined in ./ansible/roles/logstash/files/logstash.conf)
- cloud:
    IP: `11.0.0.10`
    Runs:
    - Elasticsearch:
        Runs on port `9200` (defined in ./ansible/roles/elasticsearch/files/elasticsearch.yml)
        Single cluster, single node; discoverable by master and cloud
    - Kibana:
        Runs on port `5601` (defined in ./ansible/roles/kibana/files/kibana.yml)
        Connects to `11.0.0.1:9200 elasticsearch@cloud` (defined in ./ansible/roles/kibana/files/kibana.yml)

Purpose of the other software is generating a mockup environment.
`common`, `elastic_apt`, `kibana`, `elasticsearch`, `logstash`, `filebeat_master`, `filebeat_slave`, and `metricbeat` roles are sufficient for setting up ELK Stack.


TLDR;

IP addresses:
slave: 10.0.0.10
master: 10.0.0.11
cloud: 11.0.0.10

Ports:
Kibana: 5601 (user-defined default)
Elasticsearch: 9200 (user-defined default)
Logstash: 9004 (user-defined)