## Usage
- Execute `vagrant up` in project directory.
- Launch Kibana in your browser by typing `11.0.0.10:5601` to address field.
- Create index patterns `*metricbeat*` and `*logstash*`
- Metrics dashborads should be available in Dashboards panel on the left sidebar.
- Logs should be available on Discover panel.

## Network Information
This mockup environment uses 3 ubuntu/bionic64 virtual machines.
- **puppet:**<br>
    IP: `10.0.0.10`<br>
    Runs:
    - Filebeat:<br>
        Sends syslog to `10.0.0.11:9004 Logstash@master` (defined in ./ansible/roles/filebeat_puppet/files/filebeat.yml)<br>
    - Metricbeat:<br>
        Sends metric data to `10.0.0.11:9004 Logstash@master` (defined in ./ansible/roles/metribeat/files/metricbeat.yml)<br>
        Sets Kibana dashboards up
- **master:**<br>
    IP: `10.0.0.11`<br>
    Runs:
    - Filebeat:<br>
        Sends syslog to `10.0.0.11:9004 Logstash@master` (defined in ./ansible/roles/filebeat_master/files/filebeat.yml)
    - Metricbeat:<br>
        Sends metric data to `10.0.0.11:9004 Logstash@master` (defined in ./ansible/roles/metribeat/files/metricbeat.yml)<br>
        Sets Kibana dashboards up
    - Logstash:<br>
        Listens port `9004`<br>
        Sends filtered logs and plain metrics to `11.0.0.10:9200 elasticsearch@cloud` (defined in ./ansible/roles/logstash/files/logstash.conf)
- **cloud:**<br>
    IP: `11.0.0.10`<br>
    Runs:<br>
    - Elasticsearch:<br>
        Runs on port `9200` (defined in ./ansible/roles/elasticsearch/files/elasticsearch.yml)<br>
        Single cluster, single node; discoverable by master and cloud
    - Kibana:<br>
        Runs on port `5601` (defined in ./ansible/roles/kibana/files/kibana.yml)<br>
        Connects to `11.0.0.1:9200 elasticsearch@cloud` (defined in ./ansible/roles/kibana/files/kibana.yml)

Purpose of the other software is generating a mockup environment.<br>
`common`, `elastic_apt`, `kibana`, `elasticsearch`, `logstash`, `filebeat_master`, `filebeat_puppet`, and `metricbeat` roles are sufficient for setting up ELK Stack.


TLDR;

IP addresses:<br>
puppet: 10.0.0.10<br>
master: 10.0.0.11<br>
cloud: 11.0.0.10<br>

Ports:<br>
Kibana: 5601 (user-defined default)<br>
Elasticsearch: 9200 (user-defined default)<br>
Logstash: 9004 (user-defined)<br>


## # TO DO
- Elasticsearch system configuration. (https://www.elastic.co/guide/en/elasticsearch/reference/current/system-config.html)
- Security configuration. (username and password, SSL authentication)
- _Collect which metrics?_