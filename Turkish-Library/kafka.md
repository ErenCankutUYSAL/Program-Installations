## CentOS 3 üzerinde 7 düğümleri olan bir Kafka kümesi oluşturun
Alexander Braun tarafından 17 Şub 2018 tarihinde yayınlandı - Linux, Java, Apache Kafka ile etiketlendi
Apache Kafka açık kaynaklı dağıtılmış bir akış işleme platformudur. Üst düzey bir perspektiften Kafka, üreticilerin bir konuya mesaj göndermesine ve tüketicilerin bir konudan mesaj okumasına izin veren dağıtılmış bir mesajlaşma sistemidir. Kafka büyük ölçüde ölçeklenebilir ve bir kümede çalıştırıldığında yüksek verim ve düşük gecikme sunar. Bu yazı, bir geliştirme ortamı için 3 düğümlerinden oluşan bir Kafka kümesinin nasıl kurulacağını açıklar.


 
##### Önkoşullar
Bir 3 düğüm Kafka kümesi kuracağımız için en son güncellemelere ve JDK 1.8'e sahip 3 CentOS 7 Linux sunucularına ihtiyacımız var. Bu örnekte 3 sanal makineleri kullanıyorum. Bu yazıda böyle bir Sanal Kutu görüntüsü oluşturma işlemini bulabilirsiniz.

##### Ağ Tasarımı
Aşağıdaki şema, kümeyi kurmak için kullanacağımız ağ tasarımını göstermektedir. Sanal makinelerin ana bilgisayar adları kafka1, kafka2 ve kafka3'tür. Ayrıca her sunucu için sabit bir IP adresi atarız (bir sonraki bölüme bakın). Ayrıca, küme düğümlerinin birbirleriyle iletişim kurabilmesini sağlamak için güvenlik duvarı kuralları oluşturacağız. Gerekli limanlar ZooKeeper için 2888, 3888 ve 2181 ve Kafka için 9092'dir.

Kafka Küme Ağ Tasarımı

##### Statik IP Adresleri Atama
Kümeyi kurmak için küme düğümlerimize sabit IP adresleri atamalıyız. Bu, sonraki adımlarda açıklanan ZooKeeper ve Kafka yapılandırmalarını basitleştirir.

Her sunucu için sabit bir IP adresi atamanın en kolay yolu yönlendiricimize bir DHCP rezervasyon kuralı eklemektir. Yalnızca sanal makine ağ arabiriminin MAC adresine ihtiyacımız var. Sunucuların her birinde ip a uygulayarak MAC adresini alabiliriz.

```
[user@kafka1 ~]$ ip a
1: lo:  mtu 65536 qdisc noqueue state UNKNOWN qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3:  mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 08:00:27:74:3a:f4 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.101/24 brd 192.168.1.255 scope global dynamic enp0s3
       valid_lft 86389sec preferred_lft 86389sec
    inet6 2001:569:70df:8b00:8728:ced9:1c2a:387f/64 scope global noprefixroute dynamic
       valid_lft 14696sec preferred_lft 14396sec
    inet6 fe80::ae37:4c20:e5bd:8014/64 scope link
       valid_lft forever preferred_lft forever
	   
```
Yukarıdaki örnek, sanal makinenin MAC adresi 08:00:27:74: 3a: f4 kullandığını göstermektedir. Artık yönlendiriciye bir DHCP kuralı atamak için MAC adresini kullanabiliriz. Aşağıdaki resim tüm sunucu düğümleri için kuralları göstermektedir.

Statik IP Adresleri DHCP Kuralı

Yönlendiriciye erişiminiz yoksa ve bir DHCP kuralı ayarlayamıyorsanız, burada açıklandığı gibi statik bir IP adresi yapılandırabilirsiniz.

##### /etc/hosts dosyasını ayarlayın
Yapılandırma dosyalarında genellikle sabit IP adresleri yerine ana bilgisayar adlarını kullanmayı tercih ederim. Bunu herkese açık alan adları olmadan başarmak için, sunucu adlarını her sunucudaki /etc/hosts dosyasına eklemeliyiz.
```
[user@kafka1 ~]$ sudo vi /etc/hosts
```
Aşağıdaki girdileri ekleyelim:
```
192.168.1.101   kafka1
192.168.1.102   kafka2
192.168.1.103   kafka3
```

##### Güvenlik duvarı kuralları oluşturun
Yukarıda belirtildiği gibi, istemcilerin Kafka kümemize bağlanmasına ve düğümlerin birbirleriyle iletişim kurmasına izin vermek için birkaç bağlantı noktası açmamız gerekiyor. CentOS 7'de ilgili güvenlik duvarı kurallarını eklememiz gerekiyor.

###### ZooKeeper güvenlik duvarı kuralı
ZooKeeper için güvenlik duvarı kuralıyla başlayalım. 2888, 3888 ve 2181 portlarını açmamız gerekiyor.
```
[user@kafka1 ~]$ sudo vi /etc/firewalld/services/zooKeeper.xml
```
Aşağıdaki içeriği zooKeeper.xml dosyasına eklemeliyiz.
```
<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>ZooKeeper</short>
  <description>Firewall rule for ZooKeeper ports</description>
  <port protocol="tcp" port="2888"/>
  <port protocol="tcp" port="3888"/>
  <port protocol="tcp" port="2181"/>
</service>
```
###### Kafka güvenlik duvarı kuralı
Kafka için 9092 portunu açmamız gerekiyor.
```
[user@kafka1 ~]$ sudo vi /etc/firewalld/services/kafka.xml
```
Aşağıdaki içeriği kafka.xml dosyasına eklememiz gerekiyor.
```
<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>Kafka</short>
  <description>Firewall rule for Kafka port</description>
  <port protocol="tcp" port="9092"/>
</service>
```
##### Yeni kuralları etkinleştirin
Artık yeni güvenlik duvarı kurallarını etkinleştirebiliriz. Mevcut tüm hizmet özelliklerinin yeniden yüklenmesini zorunlu kılmak için önce firewalld hizmetini yeniden başlatalım.
```
[user@kafka1 ~]$ sudo service firewalld restart
```
Artık ZooKeeper ve Kafka için güvenlik duvarı kurallarını kalıcı olarak ekleyebiliriz.
```
[user@kafka1 ~]$ sudo firewall-cmd --permanent --add-service=zooKeeper
[user@kafka1 ~]$ sudo firewall-cmd --permanent --add-service=kafka
```
Kuralları etkinleştirdikten sonra firewalld'yi yeniden başlatmamız gerekiyor.
```
[user@kafka1 ~]$ sudo service firewalld restart
```
Her şeyin beklendiği gibi çalıştığından emin olmak için yeni hizmetlerin etkinleştirilip etkinleştirilmediğini kontrol edebiliriz.
```
[user@kafka1 ~]$ sudo firewall-cmd --list-services
ssh dhcpv6-client ntp zooKeeper kafka
```
Çıktı, her iki hizmetin de etkin olduğunu gösterir.

##### Kafka kullanıcısı oluştur
Kafka düğümünü çalıştırmak için ayrı bir kafka kullanıcısı oluşturmanızı öneririm.
```
[user@kafka1 ~]$ sudo adduser kafka
[user@kafka1 ~]$ sudo passwd kafka
```
Kullanıcıyı oluşturduktan sonra yeni kullanıcıya geçebilir ve kalan yapılandırmayı gerçekleştirebiliriz.
```
[user@kafka1 ~]$ su kafka
```
##### ZooKeeper ve Kafka'yı yükleyin
Artık Apache Kafka'yı indirebiliriz
```
[kafka@kafka1 ~]$ wget http://apache.forsale.plus/kafka/1.0.0/kafka_2.11-1.0.0.tgz
```
tar.gz dosyasını açalım
```
[kafka@kafka1 ~]$ tar -xzf kafka_2.11-1.0.0.tgz
```

Bu kadar! Kafka arşivi zaten ZooKeeper içerdiğinden, sistemimizde zaten tam olarak çalışan bir Kafka kurulumuna sahibiz. Bir kümeyi çalıştırmak için konfigürasyonu ayarlamamız gerekir.

##### Dizinler oluşturun
ZooKeeper ve Kafka için veri ve günlük dizinleri oluşturmamız gerekiyor. Bu işlemi basitleştirmek için kullanıcı ana dizini içindeki dizinleri ekleyebiliriz. Bir üretim ortamında farklı konumlar kullanırdık, ör. veri ve günlük dizinleri için ayrı bağlama noktaları veya fiziksel diskler.
```
[kafka@kafka1 ~]$ mkdir -p /home/kafka/zookeeper/data
[kafka@kafka1 ~]$ mkdir -p /home/kafka/kafka/kafka-logs
```
##### ZooKeeper yapılandırması

Katıştırılmış ZooKeeper örneğinin yapılandırma dosyası kafka_2.11-1.0.0/config/zookeeper.properties konumunda bulunur.
```
[kafka@kafka1 ~]$ vi kafka_2.11-1.0.0/config/zookeeper.properties
```
Bu dosya içerisinde dataDir özelliğini bulmalı ve değeri yukarıda oluşturduğumuz yeni ZooKeeper dizinine işaret edecek şekilde ayarlamalıyız.
```
dataDir=/home/kafka/zookeeper/data
```
Bu dosyanın sonunda mevcut tüm ZooKeeper sunucularını eklememiz gerekiyor. Ayrıca initLimit ve syncLimit özelliklerini de ekliyoruz.
```
server.1=kafka1:2888:3888
server.2=kafka2:2888:3888
server.3=kafka3:2888:3888

initLimit=5
syncLimit=2
```
Küme düğümlerimizin her biri benzersiz bir sunucu kimliğine ihtiyaç duyar. ZooKeeper bu bilgiyi şu dosyadan arar: /home/kafka/zookeeper/data/myid. Her sunucuda bunun gibi bir komut yürütmemiz gerekiyor - her örnek için farklı bir değer kullanarak. Örneğin, sunucu kafka1'de 1 "1" değerini kullanıyoruz.
```
[kafka@kafka1 ~]$ echo "1" > /home/kafka/zookeeper/data/myid
```
##### Apache Kafka yapılandırması
Şimdi burada saklanan Kafka yapılandırma dosyalarını ayarlayabiliriz: kafka_2.11-1.0.0/config/server.properties.
```
[kafka@kafka1 ~]$ vi kafka_2.11-1.0.0/config/server.properties
```
ZooKeeper yapılandırmasına benzer şekilde, her Kafka küme düğümü benzersiz bir kimliğe ihtiyaç duyar. Yapılandırma dosyasında broker.id özelliğini bulmalı ve her sunucu için kimliği değiştirmeliyiz. 1, 2 ve 3'ü kullanmanızı tavsiye ederim.
```
broker.id=1
```
Ayrıca log.dirs parametresinde belirtilen log dizini konumunu da değiştirmemiz gerekiyor.
```
log.dirs=/home/kafka/kafka/kafka-logs
```
Ek olarak, dinleyicileri ve reklamı yapılan.listeners özelliklerini mevcut Kafka düğümü ana bilgisayar adıyla güncellememiz gerekiyor.

dinleyiciler: kafka'nın dinlediği adres/sunucu adı ve protokol (Kafka düğümleri arasındaki dahili trafik)
reklamı yapılan.listener: istemcilerin Kafka kümesine (harici trafik) bağlanmak için kullanabileceği adres/sunucu adı ve protokol. Yalnızca yukarıdaki ayardan farklıysa belirtilmesi gerekir.
```
listeners=PLAINTEXT://kafka1:9092
advertised.listeners=PLAINTEXT://kafka1:9092
```
Bir sonraki adım, Kafka'ya hangi ZooKeeper düğümlerinin bağlanmak için kullanılabileceğini söylemektir. Özellikle yüzlerce düğüm içeren büyük bir kümeyi çalıştırırken, mevcut tüm sunucu düğümlerinin buraya eklenmesi gerekmez. Birkaç tohum düğümü eklemek yeterlidir. Kafka, mevcut tüm düğümleri tanımlar ve yeni düğümler kümeye katılır veya kümeden ayrılırsa mevcut düğümleri günceller.
```
zookeeper.connect=kafka1:2181,kafka2:2181,kafka3:2181
```
Bir geliştirme ortamında, genellikle delete.topic.enable özelliğini eklerim. Bu özelliği true olarak ayarlamak, çalışma zamanında konuları kolayca silmemizi sağlar. Bu özellik ayarlanmazsa, Kafka yalnızca konuları silinmiş olarak işaretler.
```
delete.topic.enable=true
```
İşte bu, 3 düğümlü Kafka kümemizi yapılandırdık!

##### Küme kurulumunu başlatın ve test edin
Sonunda ZooKeeper ve Kafka'yı başlatabilir ve hızlı bir test yapabiliriz.

###### ZooKeeper'ı Başlatın
ZooKeeper'ı başlatmak için her düğümde aşağıdaki komutu yürütün:
```
[kafka@kafka1 ~]$ cd kafka_2.11-1.0.0
[kafka@kafka1 ~]$ nohup bin/zookeeper-server-start.sh config/zookeeper.properties &
```
###### Apache Kafka'yı başlatın

Şimdi Kafka'yı başlatabiliriz:
```
[kafka@kafka1 ~]$ nohup bin/kafka-server-start.sh config/server.properties &
```
###### Yeni bir konu oluşturun

Kurulumu test etmek için bir konu oluşturmalıyız.
```
[kafka@kafka1 ~]$ bin/kafka-topics.sh --create --zookeeper kafka1:2181,kafka2:2181,kafka3:2181 --replication-factor 1 --partitions 6 --topic topic1 --config cleanup.policy=delete --config delete.retention.ms=60000
```
Yukarıdaki komut, konu1 adında 6 bölümlü yeni bir konu oluşturur. Her küme 2 bölümden sorumlu olacaktır. Çoğaltma faktörü 1'e ayarlanmıştır; bu, verilerin çoğaltılmadığı ve belirli bir bölüme ilişkin verilerin yalnızca bir sunucuda depolanacağı anlamına gelir.

Ayrıca mevcut tüm konuların bir listesini alabiliriz
```
[kafka@kafka1 ~]$ bin/kafka-topics.sh --list --zookeeper kafka1:2181
```
Ve konumuzla ilgili ayrıntılı bir açıklama alabiliriz.
```
[kafka@kafka1 ~]$ bin/kafka-topics.sh --describe --zookeeper kafka1:2181 --topic topic1
```
Benim durumumda yukarıdaki komut yazdırılır:
```
Topic:topic1	PartitionCount:6	ReplicationFactor:1	Configs:delete.retention.ms=60000,cleanup.policy=delete
	Topic: topic1	Partition: 0	Leader: 2	Replicas: 2	Isr: 2
	Topic: topic1	Partition: 1	Leader: 3	Replicas: 3	Isr: 3
	Topic: topic1	Partition: 2	Leader: 1	Replicas: 1	Isr: 1
	Topic: topic1	Partition: 3	Leader: 2	Replicas: 2	Isr: 2
	Topic: topic1	Partition: 4	Leader: 3	Replicas: 3	Isr: 3
	Topic: topic1	Partition: 5	Leader: 1	Replicas: 1	Isr: 1
```
Komut, hangi sunucunun hangi bölümden sorumlu olduğunu ve hangi sunucunun verileri çoğalttığını gösterir.

###### Kümeyi test edin
Kafka paketi, kümenin çalışıp çalışmadığını kontrol etmek için kullanılabilecek bir üretici ve bir tüketici oluşturmak için zaten iki komut satırı aracı içerir.

Üreticiyi sunucularımızdan birinde başlatabiliriz. Komut bir istem açar ve buraya girdiğimiz her şey konuya gönderilir.
```
[kafka@kafka1 ~]$ bin/kafka-console-producer.sh --broker-list kafka1:9092 --topic topic1
```
Artık sunucularımızdan birinde bir tüketici başlatabiliriz.
```
[kafka@kafka1 ~]$ bin/kafka-console-consumer.sh --bootstrap-server kafka1:9092 --topic topic1
```
Üretici istemine bir şey girdiğimizde, tüketici terminalimizde yazdırılacaktır.

Bu, testimizin başarılı olduğu, Kafka kümesinin kurulduğu anlamına gelir. Önümüzdeki birkaç hafta içinde, Java'daki somut örnekler de dahil olmak üzere daha fazla Kafka örneği ve kullanım örnekleri ekleyeceğim.