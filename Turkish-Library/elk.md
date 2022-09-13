Kaynak1-Kibana Kurulumu # https://computingforgeeks.com/how-to-install-elk-stack-on-centos-fedora/

Kaynak2-FileBeat (NetFlow Kurulumu) # https://www.pluralsight.com/guides/setup-netflow-monitoring-elasticsearch-siem

## Kibana Kurulumu

# Java Kurulum.

```
sudo yum -y install java-openjdk-devel java-openjdk
```

# Elasticsearch 7 repo oluşturulması

```
cat <<EOF | sudo tee /etc/yum.repos.d/elasticsearch.repo
[elasticsearch-7.x]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
EOF
```

# Elasticsearch - Key kurulumu

```
sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```

# Belleğin Temizlenmesi

```
sudo yum clean all
sudo yum makecache
```

# Elasticsearch Kurulumu

```
sudo yum -y install elasticsearch
```

# Versiyon Kontrollerinin Sağlanması

```
rpm -qi elasticsearch
```

# Bellek Ayarlanması (Başında Boşluk Olmamalı.)

```
/etc/elasticsearch/jvm.options

-Xms2g
-Xmx2g
```

# Elasticsearch Enable Edilir (Başlangıçta çalışması için)

```
sudo systemctl enable --now elasticsearch.service 
```

# Kibana Kurulumu

```
sudo yum -y install kibana
```

# Kibana.yml dosyasının düzenlenmesi

```
sudo vim /etc/kibana/kibana.yml

server.host: "0.0.0.0"
server.name: "kibana.example.com"
elasticsearch.url: "http://0.0.0.0:9200"
```

# Kibana Enable Edilir (Başlangıçta çalışması için) 

```
systemctl enable --now kibana
```

# Firewall Portlarının aktif hale getirilmesi

```
sudo firewall-cmd --add-port=5601/tcp --permanent
sudo firewall-cmd --reload
```

# Logstash Kurulumu

```
sudo yum -y install logstash
```

# Dashboard için gerekli bağımlılıkların indirilmesi. (FileBeat-NetFlow gibi)

```
sudo yum install filebeat auditbeat metricbeat packetbeat heartbeat-elastic
```

## FileBeat Kurulumu

# Data Eklenmesi

```
https://10.0.1.59:5601 ile ekrana gidilir (Add Data Denir.)
Sonra Arama alanından NetFlow/IPFIX Collector Başlığı bulunur.
Açılan ekranda adım 1 atlanılarak Adım 2 den itibaren istinelenler yapılır.
Önemli=Adım 2 içerisinde 
Output.elasticsearch Hosts: ["0.0.0.0:9200"] Olarak ayarlanır.

Setup.kibana host:"10.0.1.59:5601" olarak ayarlanır.
```

# FileBeat Modül aktiv etme

```
cd /etc/filebeat/modules.d 

filebeat modules enable netflow (ile modül çalıştırılır.)
```

# FileBeat başlatılması.

```
filebeat setup
systemctl enable filebeat
systemctl start filebeat
```

# Browserda açık olan sayfa üzerinde NetFlow Overview butonuna basılır.


## Elasticsearch Kayıtlarının Tutulduğu Yer

```
cd /var/lib/elasticsearch/nodes/0/indices
```





napoli2 napoli4 filevbeat var eski versiyon

filebeat logstashe yollar 116 da file beatleri ayarla 115 e yollayacak 







