# influxdb-grafana-docker

## Requirements
- Docker Engine
- Docker Compose

## Deploy the service
- Add storage devices for persistent storage for ``influxdb`` and ``grafana``.
- `` # docker-compose up -d``

## Configure Influxdb
Execute the following commands from the node where influxdb container is running

- Create influxdb database
 `` curl -G http://localhost:8086/query --data-urlencode "q=CREATE DATABASE collectd" ``
- Verify database 
  `` curl -G 'http://localhost:8086/query?pretty=true' --data-urlencode "q=SHOW DATABASES" ``
- 
```
echo "net.core.rmem_max=26214400" >> /etc/sysctl.conf
echo "net.core.rmem_default=26214400" >> /etc/sysctl.conf
sysctl -p /etc/sysctl.conf
```

## Configure Grafana 
Execure the following commands from the node where Grafana conatiner is running

- Add Grafana datasource

 ``` curl 'http://admin:admin@127.0.0.1:3000/api/datasources' -X POST -H 'Content-Type: application/json;charset=UTF-8' --data-binary '{"name":"influxdb","type":"influxdb","url":"http://influxdb:8086","access":"proxy","isDefault":true,"database":"collectd"}' ```

- Verify Grafana Datasource

  `` curl 'http://admin:admin@127.0.0.1:3000/api/datasources' ``

## Setup Collectd
 `` yum install -y collectd ``
- Configure collectd 
```
wget https://raw.githubusercontent.com/deniszh/collectd-iostat-python/master/collectd_iostat_python.py -O /usr/lib64/collectd/collectd_iostat_python.py

chmod 755 /usr/lib64/collectd/collectd_iostat_python.py

wget https://raw.githubusercontent.com/deniszh/collectd-iostat-python/master/iostat_types.db -O /usr/share/collectd/iostat_types.db

```
