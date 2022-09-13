## CentOS 7'de Graylog Sunucusu Nasıl Kurulur

Graylog sunucusu, kurumsal kullanıma hazır, açık kaynaklı bir günlük yönetimi yazılım paketidir. Çeşitli kaynaklardan günlükleri toplar ve sorunları keşfetmek ve çözmek için bunları analiz eder. Graylog sunucusu temel olarak Elasticsearch, MongoDB ve Graylog'un birleşimidir. Elasticsearch, metin depolamak ve çok güçlü arama yetenekleri sağlamak için çok popüler bir açık kaynak uygulamasıdır. MongoDB, verileri NoSQL biçiminde depolamak için açık kaynaklı bir uygulamadır. Graylog, çeşitli kaynaklardan günlükleri toplar ve günlükleri yönetmek ve aramak için web tabanlı bir gösterge panosu sağlar. Graylog ayrıca hem yapılandırma hem de veriler için bir REST API sağlar. Tek bir merkezi konumdan alan istatistiklerini, hızlı değerleri ve çizelgeleri kullanarak metrikleri görselleştirmek ve eğilimleri gözlemlemek için kullanılabilecek yapılandırılabilir bir gösterge panosu sağlar.

Bu öğreticide, Graylog Server'ı CentOS 7'ye kurmayı öğreneceksiniz. Bu kılavuz Graylog Server 2.3 için yazılmıştır, ancak daha yeni sürümlerde de çalışabilir. Ayrıca Java, Elasticsearch ve MongoDB kurmayı da öğreneceksiniz. Ayrıca MongoDB örneğinin güvenliğini sağlayacağız ve web tabanlı kontrol paneli ve API için bir Nginx ters proxy kuracağız.

### Önkoşullar
En az 4 GB RAM'e sahip bir Vultr CentOS 7 sunucu örneği.
Bir sudo kullanıcısı.


### Java'yı yükleyin
Elasticsearch'ün çalışması için Java 8 gerekir. Hem Oracle Java'yı hem de OpenJDK'yı destekler, ancak mümkün olduğunda her zaman Oracle Java kullanmanız önerilir. Oracle, kuruluma hazır RPM paketleri sağlar. Oracle JDK RPM'yi indirin:

```
wget --no-cookies --no-check-certificate --header "Cookie:oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u144-b01/090f390dda5b47b9b721c7dfaa008135/jdk-8u144-linux-x64.rpm"
```
RPM paketini kurun.

```
sudo yum -y install jdk-8u144-linux-x64.rpm
```

Java başarıyla yüklendiyse, sürümünü doğrulayabilmeniz gerekir.

```
java -version
```

Aşağıdaki çıktıyı göreceksiniz.

```
[user@vultr ~]$ java -version
java version "1.8.0_144"
Java(TM) SE Runtime Environment (build 1.8.0_144-b01)
Java HotSpot(TM) 64-Bit Server VM (build 25.144-b01, mixed mode)
```
```Java_HOME``` ve ```JRE_HOME``` ortam değişkenini aşağıdakileri çalıştırarak ayarlayın:

```
echo "export JAVA_HOME=/usr/java/jdk1.8.0_144/" >> ~/.bash_profile
echo "export JRE_HOME=/usr/java/jdk1.8.0_144/jre" >> ~/.bash_profile
```

Şimdi, aşağıdaki komutu kullanarak dosyayı yönlendirin.
```
source ~/.bash_profile
```
Ortam değişkeninin ayarlanıp ayarlanmadığını kontrol etmek için echo $Java_HOME komutunu çalıştırın.
```
[user@vultr ~]$ echo $JAVA_HOME
/usr/java/jdk1.8.0_144/
```

### Elasticsearch'ü yükleyin

Elasticsearch, günlükleri depolamak ve bunlar arasında arama yapmak için kullanılan, dağıtılmış, gerçek zamanlı, ölçeklenebilir ve yüksek oranda kullanılabilir bir uygulamadır. Verileri indekslerde saklar ve veriler arasında arama yapmak çok hızlıdır. HTTP RESTful API ve yerel Java API gibi çeşitli API kümeleri sağlar. Elasticsearch, doğrudan Elasticsearch deposu aracılığıyla kurulabilir. Elasticsearch için yeni bir depo dosyası oluşturun.
```
sudo nano /etc/yum.repos.d/elasticsearch.repo
```
Dosyayı aşağıdaki içerikle doldurun.
```
[elasticsearch-5.x]
name=Elasticsearch repository for 5.x packages
baseurl=https://artifacts.elastic.co/packages/5.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

Paketleri imzalamak için kullanılan PGP anahtarını içe aktarın. Bu, paketlerin bütünlüğünü sağlayacaktır.
```
sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```
Elasticsearch paketini kurun:
```
sudo yum -y install elasticsearch
```
Paket yüklendikten sonra Elasticsearch varsayılan yapılandırma dosyasını açın.
```
sudo nano /etc/elasticsearch/elasticsearch.yml
```
Aşağıdaki satırı bulun, yorumunu kaldırın ve değeri my-application'dan graylog'a değiştirin.
```
cluster.name: graylog
```
Elasticsearch'ü başlatabilir ve açılışta otomatik olarak başlamasını sağlayabilirsiniz:
```
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```
Elasticsearch şimdi 9200 numaralı bağlantı noktasında çalışıyor. Aşağıdakileri çalıştırarak düzgün çalıştığını doğrulayın:
```
curl -XGET 'localhost:9200/?pretty'
```
Aşağıdakine benzer bir çıktı görmelisiniz.
```
[user@vultr ~]$ curl -XGET 'localhost:9200/?pretty'
{
  "name" : "-kYzFA9",
  "cluster_name" : "graylog",
  "cluster_uuid" : "T3JQKehzSqmLThlVkEKPKg",
  "version" : {
    "number" : "5.5.1",
    "build_hash" : "19c13d0",
    "build_date" : "2017-07-18T20:44:24.823Z",
    "build_snapshot" : false,
    "lucene_version" : "6.6.0"
  },
  "tagline" : "You Know, for Search"
}
```
Hatalarla karşılaşırsanız, Elasticsearch'ün başlatma işlemini tamamlaması zaman alacağından birkaç saniye bekleyin ve yeniden deneyin. Elasticsearch şimdi yüklendi ve düzgün çalışıyor.

### MongoDB'yi yükleyin
MongoDB ücretsiz ve açık kaynaklı NoSQL veritabanı sunucusudur. Verilerini düzenlemek için tabloları kullanan geleneksel veritabanının aksine, MongoDB belge odaklıdır ve şemaları olmayan JSON benzeri belgeler kullanır. Graylog, yapılandırmasını ve meta bilgilerini depolamak için MongoDB'yi kullanır. Doğrudan MongoDB deposu aracılığıyla kurulabilir. MongoDB için yeni bir depo dosyası oluşturun.

```sudo nano /etc/yum.repos.d/mongodb.repo```

Dosyayı aşağıdaki içerikle doldurun.
```
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```
Aşağıdakileri çalıştırarak MongoDB'yi kurun:
```
sudo yum -y install mongodb-org
```
Start MongoDB server and enable it to start automatically.
```
sudo systemctl start mongod
sudo systemctl enable mongod
```
### Graylog sunucusunu kurun
Graylog sunucusu için en son depoyu indirin.
```
sudo rpm -Uvh https://packages.graylog2.org/repo/packages/graylog-2.3-repository_latest.rpm
sudo yum -y update
```
##### Graylog'u aşağıdakileri çalıştırarak yükleyin:

```
sudo yum -y install graylog-server
```

Graylog sunucusu artık sunucunuza yüklenmiştir. Başlamadan önce, birkaç şeyi yapılandırmanız gerekecek.

##### Graylog'u Yapılandırın
Güçlü parolalar oluşturmak için pwgen yardımcı programını kurun.

```
sudo yum -y install pwgen
```

Şimdi güçlü bir parola sırrı oluşturun.

```pwgen -N 1 -s 96```
Şuna benzer bir çıktı alacaksınız:
```
[user@vultr ~]$ pwgen -N 1 -s 96
pJqhNbdEY9FtNBfFUtq20lG2m9daacmsZQr59FhyoA0Wu3XQyVZcu5FedPZ9eCiDfjdiYWfRcEQ7a36bVqxSyTzcMMx5Rz8v
```
Ayrıca, root ```admin``` kullanıcısının şifresi için 256 bitlik bir hash oluşturun:

```
echo -n StrongPassword | sha256sum
```
StrongPassword'u yönetici kullanıcı için ayarlamak istediğiniz parolayla değiştirin. Göreceksin:
```
[user@vultr ~]$ echo -n StrongPassword | sha256sum
05a181f00c157f70413d33701778a6ee7d2747ac18b9c0fbb8bd71a62dd7a223  -
```
Graylog yapılandırma dosyasını açın:

```
sudo nano /etc/graylog/server/server.conf
```

```password_secret =``` bulun, ```pwgen``` komutu ile oluşturulan şifreyi kopyalayıp yapıştırın. ```root_password_sha2 =``` bulun, yönetici şifrenizin dönüştürülmüş SHA 256-bit karmasını kopyalayıp yapıştırın. ```#root_email =``` bulun, yorumunuzu kaldırın ve e-posta adresinizi girin. Yorumu kaldırın ve saat diliminizi ```root_timezone``` olarak ayarlayın. Örneğin:

```
password_secret = pJqhNbdEY9FtNBfFUtq20lG2m9daacmsZQr59FhyoA0Wu3XQyVZcu5FedPZ9eCiDfjdiYWfRcEQ7a36bVqxSyTzcMMx5Rz8v
root_password_sha2 = 05a181f00c157f70413d33701778a6ee7d2747ac18b9c0fbb8bd71a62dd7a223
root_email = mail@example.com
root_timezone = Asia/Kolkata
```

```#web_enable = false``` yorumunu kaldırarak ve değeri ```true``` olarak ayarlayarak web tabanlı Graylog arayüzünü etkinleştirin. Ayrıca, aşağıdaki satırları belirtildiği gibi kaldırın ve değiştirin.

```
rest_listen_uri = http://0.0.0.0:9000/api/
rest_transport_uri = http://45.76.214.19:9000/api/
web_enable = true
web_listen_uri = http://0.0.0.0:9000/
```
Dosyayı kaydedin ve metin düzenleyicinizden çıkın.

Aşağıdakileri çalıştırarak Graylog hizmetini yeniden başlatın:
```
sudo systemctl restart graylog-server
```
### Nginx'i ters proxy olarak yapılandırın
Varsayılan olarak, Graylog web arayüzü, 9000 numaralı bağlantı noktasındaki localhost'u dinler ve API, URL /api ile 9000 numaralı bağlantı noktasını dinler. Bu öğreticide, uygulamaya standart HTTP bağlantı noktası üzerinden erişilebilmesi için Nginx'i ters proxy olarak kullanacağız. Aşağıdakileri çalıştırarak Nginx web sunucusunu kurun:
```
sudo yum -y install nginx
```
Yazarak varsayılan sanal ana bilgisayarı açın.
```
sudo nano /etc/nginx/nginx.conf
```
`http` altındaki `sunucu` bloğunu bulun ve tüm `sunucu` bloğunu aşağıdaki satırlarla değiştirin.
```
server
{
    listen 80 default_server;
    listen [::]:80 default_server ipv6only=on;
    server_name graylog.example.com 192.0.2.1;

    location / {
      proxy_set_header Host $http_host;
      proxy_set_header X-Forwarded-Host $host;
      proxy_set_header X-Forwarded-Server $host;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Graylog-Server-URL http://$server_name/api;
      proxy_pass       http://127.0.0.1:9000;
    }
}
```
Nginx'i başlatın ve açılışta otomatik olarak başlamasını sağlayın:
```
sudo systemctl start nginx
sudo systemctl enable nginx
```
### Güvenlik duvarını ve SELinux'u yapılandırın
Sunucunuzda bir güvenlik duvarı çalıştırıyorsanız, belirli bağlantı noktaları için bir istisna ayarlamak üzere güvenlik duvarını yapılandırmanız gerekir. Elasticsearch hizmetinin ve Nginx ters proxy'nin ağın dışından bağlanmasına izin verin.
```
sudo firewall-cmd --zone=public --permanent --add-service=http
sudo firewall-cmd --zone=public --permanent --add-port=9200/tcp
sudo firewall-cmd --reload
```
Sisteminizde SELinux'u etkinleştirdiyseniz, SELinux ilkelerine birkaç istisna eklemeniz gerekecektir.
```
sudo setsebool -P httpd_can_network_connect 1
sudo semanage port -a -t http_port_t -p tcp 9000
sudo semanage port -a -t http_port_t -p tcp 9200
sudo semanage port -a -t mongod_port_t -p tcp 27017
```
### Çözüm
Graylog sunucusunun kurulumu ve temel yapılandırması artık tamamlanmıştır. Artık Graylog sunucusuna ```http://192.0.2.1``` veya DNS yapılandırdıysanız ````http://graylog.example.com``` adresinden erişebilirsiniz. Daha önce ``root_password_sha2`` için belirlediğiniz şifrenin ```admin``` kullanıcı adını ve düz metin sürümünü kullanarak giriş yapın.

Tebrikler - CentOS 7 sunucunuzda tam olarak çalışan bir Graylog sunucunuz var.