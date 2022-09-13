#### Tanıtım
MongoDB, ücretsiz ve açık kaynaklı, belge odaklı bir veritabanıdır. Geleneksel tablo tabanlı ilişkisel veritabanı yapısına dayanmadığı için NoSQL veritabanı olarak sınıflandırılır. Bunun yerine, dinamik şemalara sahip JSON benzeri belgeler kullanır. İlişkisel veritabanlarından farklı olarak MongoDB, bir veritabanına veri eklemeden önce önceden tanımlanmış bir şema gerektirmez. Güncellenmiş bir şema ile yeni bir veritabanı kurmak zorunda kalmadan, şemayı istediğiniz zaman ve gerektiği sıklıkta değiştirebilirsiniz.

Bu eğitim, bir CentOS 7 sunucusuna MongoDB Community Edition kurulumunda size rehberlik eder.

#### Önkoşullar
Bu öğreticiyi izlemeden önce, sudo ayrıcalıklarına sahip normal, root olmayan bir kullanıcıya sahip olduğunuzdan emin olun. Bu ayrıcalıklara sahip bir kullanıcının nasıl kurulacağı hakkında daha fazla bilgiyi CentOS'ta Sudo Kullanıcısı Nasıl Oluşturulur adlı kılavuzumuzdan öğrenebilirsiniz.

##### Adım 1 – MongoDB Deposunu Ekleme
mongodb-org paketi, CentOS için varsayılan depolarda mevcut değildir. Ancak, MongoDB özel bir depoya sahiptir. Sunucumuza ekleyelim.

Vi düzenleyiciyle, CentOS için paket yönetimi yardımcı programı olan yum için bir .repo dosyası oluşturun:
```
sudo vi /etc/yum.repos.d/mongodb-org.repo
```
Ardından, MongoDB belgelerinin Red Hat'e Yükle bölümünü ziyaret edin ve dosyaya en son kararlı sürüm için depo bilgilerini ekleyin:
```
/etc/yum.repos.d/mongodb-org.repo
```
```
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```
Dosyayı kaydedin ve kapatın.


Devam etmeden önce, MongoDB deposunun yum yardımcı programında var olduğunu doğrulamalıyız. Repolist komutu, etkinleştirilmiş depoların bir listesini görüntüler:
```
yum repolist
```
```
Output

repo id                          repo name
base/7/x86_64                    CentOS-7 - Base
extras/7/x86_64                  CentOS-7 - Extras
mongodb-org-3.2/7/x86_64         MongoDB Repository
updates/7/x86_64                 CentOS-7 - Updates
```
MongoDB Deposu yerindeyken, kuruluma devam edelim.

##### 2. Adım – MongoDB'yi Yükleme
Mongodb-org paketini yum yardımcı programını kullanarak üçüncü taraf deposundan kurabiliriz.
```
sudo yum install mongodb-org
```
İki tane Bu tamam mı ```[y/N]:``` istemi var. Birincisi MongoDB paketlerinin kurulumuna izin verir ve ikincisi bir GPG anahtarını içe aktarır. MongoDB'nin yayıncısı yazılımlarını imzalar ve yum, indirilen paketlerin bütünlüğünü doğrulamak için bir anahtar kullanır. Her istemde Y yazın ve ardından ENTER tuşuna basın.

Ardından, systemctl yardımcı programıyla MongoDB hizmetini başlatın:
```
sudo systemctl start mongod
```
Bunları bu eğitimde kullanmayacağız olsa da, yeniden yükle ve durdur komutları ile MongoDB hizmetinin durumunu da değiştirebilirsiniz.

reload komutu, mongod işleminin ```/etc/mongod.conf``` yapılandırma dosyasını okumasını ve yeniden başlatma gerektirmeden tüm değişiklikleri uygulamasını ister.
```
sudo systemctl reload mongod
```
Stop komutu, çalışan tüm mongod işlemlerini durdurur.
```
sudo systemctl stop mongod
```
systemctl yardımcı programı start komutunu yürüttükten sonra bir sonuç vermedi ancak tail komutuyla mongod.log dosyasının sonunu görüntüleyerek hizmetin başladığını kontrol edebiliriz:
```
sudo tail /var/log/mongodb/mongod.log
```
```
Output

[initandlisten] waiting for connections on port 27017
```
Bir bağlantı bekleme çıktısı, MongoDB'nin başarıyla başladığını ve MongoDB Kabuğu ile veritabanı sunucusuna erişebileceğimizi doğrular:
```
mongo
```
<$>[not] Not: MongoDB Shell'i başlattığınızda şöyle bir uyarı görmüş olabilirsiniz:

`** WARNING: soft rlimits too low. rlimits set to 4096 processes, 64000 files. Number of processes should be at least 32000 : 0.5 times number of files.`

MongoDB dişli bir uygulamadır. İş yükünü idare etmek için ek işlemler başlatabilir. Uyarı, MongoDB'nin en etkili olabilmesi için, başlatmaya yetkili olduğu işlem sayısının herhangi bir zamanda açabileceği dosya sayısının yarısı kadar olması gerektiğini belirtir. Uyarıyı gidermek için, ```20-nproc.conf``` dosyasını düzenleyerek "mongod" için işlemlerin yumuşak sınır değerini değiştirin:
```
sudo vi /etc/security/limits.d/20-nproc.conf
```
Dosyanın sonuna aşağıdaki satırı ekleyin:
```
. . .
mongod soft nproc 32000
```
Yeni sınırın MongoDB için kullanılabilir olması için systemctl yardımcı programını kullanarak onu yeniden başlatın:
```
sudo systemctl restart mongod
```
Daha sonra MongoDB Kabuğuna bağlandığınızda uyarının ortadan kalkması gerekir. <$>

Kabuktan MongoDB ile nasıl etkileşim kuracağınızı öğrenmek için, db nesnesi için bir yöntem listesi sağlayan db.help() yönteminin çıktısını inceleyebilirsiniz.
```
db.help()
```
```
Output
DB methods:
    db.adminCommand(nameOrDocument) - switches to 'admin' db, and runs command [ just calls db.runCommand(...) ]
    db.auth(username, password)
    db.cloneDatabase(fromhost)
    db.commandHelp(name) returns the help for the command
    db.copyDatabase(fromdb, todb, fromhost)
    db.createCollection(name, { size : ..., capped : ..., max : ... } )
    db.createUser(userDocument)
    db.currentOp() displays currently executing operations in the db
    db.dropDatabase()
```
Mongod işlemini arka planda çalışır durumda bırakın, ancak çıkış komutuyla kabuktan çıkın:
```
exit
```
```
Output
Bye
```
##### Adım 3 – Başlatmayı Doğrulama
Veritabanına dayalı bir uygulama veritabanı olmadan çalışamayacağından, MongoDB arka plan programının, mongod'un sistemle başlayacağından emin olacağız.

Başlangıç durumunu kontrol etmek için systemctl yardımcı programını kullanın:
```
systemctl is-enabled mongod; echo $?
```
Sıfır çıktısı, istediğimiz etkin bir arka plan programını onaylar. Ancak bir tanesi, başlamayan devre dışı bırakılmış bir arka plan programını onaylar.
```
Output
. . .
enabled
0
```
Devre dışı bırakılmış bir arka plan programı durumunda, etkinleştirmek için systemctl yardımcı programını kullanın:
```
sudo systemctl enable mongod
```
Artık, sistem yeniden başlatıldıktan sonra otomatik olarak başlayacak, çalışan bir MongoDB örneğine sahibiz.

##### Adım 4 – Örnek Veri Kümesini İçe Aktarma (İsteğe Bağlı)
Diğer veritabanı sunucularından farklı olarak MongoDB, test veritabanında verilerle birlikte gelmez. Üretim verilerini kullanarak yeni yazılımlarla deneme yapmak istemediğimiz için, `MongoDB` ile Başlarken belgelerinin `Örnek Veri Kümesini İçe Aktar` bölümünden bir örnek veri kümesi indireceğiz. JSON belgesi, MongoDB ile etkileşim kurma alıştırması yapmak ve hassas verilere zarar vermekten kaçınmak için kullanacağımız bir kafe koleksiyonu içerir.

Yazılabilir bir dizine geçerek başlayın:
```
cd /tmp
```
JSON dosyasını indirmek için curl komutunu ve MongoDB'den gelen bağlantıyı kullanın:
```
curl -LO https://raw.githubusercontent.com/mongodb/docs-assets/primer-dataset/primer-dataset.json
```
mongoimport komutu, verileri test veritabanına ekleyecektir. `--db` bayrağı hangi veritabanının kullanılacağını tanımlarken `--collection` bayrağı bilgilerin veritabanında nerede saklanacağını belirtir ve `--file` flag komuta, içe aktarma işlemini hangi dosyada gerçekleştireceğini söyler:
```
mongoimport --db test --collection cafe --file /tmp/primer-dataset.json
```
Çıktı, verilerin `primer-dataset.json` dosyasından içe aktarıldığını onaylar:
```
Output
connected to: localhost
imported 25359 documents
```
Örnek veri kümesi yerindeyken, ona karşı bir sorgu gerçekleştireceğiz.

MongoDB Shell'i yeniden başlatın:
```
mongo
```
Kabuk, varsayılan olarak, verilerimizi içe aktardığımız test veritabanını seçer.

Veri kümesindeki tüm restoranların bir listesini görüntülemek için cafe koleksiyonunu ``find()`` yöntemiyle sorgulayın. Koleksiyon 25.000'den fazla giriş içerdiğinden, sorgunun çıktısını belirli bir sayıya indirmek için isteğe bağlı `limit()` yöntemini kullanın. Ayrıca, `nice()` yöntemi, bilgileri yeni satırlar ve girintilerle daha insan tarafından okunabilir hale getirir.
```
db.cafe.find().limit( 1 ).pretty()
```
```
Output
{
    "_id" : ObjectId("57e0443b46af7966d1c8fa68"),
    "address" : {
        "building" : "1007",
        "coord" : [
            -73.856077,
            40.848447
        ],
        "street" : "Morris Park Ave",
        "zipcode" : "10462"
    },
    "borough" : "Bronx",
    "cuisine" : "Bakery",
    "grades" : [
        {
            "date" : ISODate("2014-03-03T00:00:00Z"),
            "grade" : "A",
            "score" : 2
        },
        {
            "date" : ISODate("2013-09-11T00:00:00Z"),
            "grade" : "A",
            "score" : 6
        },
        {
            "date" : ISODate("2013-01-24T00:00:00Z"),
            "grade" : "A",
            "score" : 10
        },
        {
            "date" : ISODate("2011-11-23T00:00:00Z"),
            "grade" : "A",
            "score" : 9
        },
        {
            "date" : ISODate("2011-03-10T00:00:00Z"),
            "grade" : "B",
            "score" : 14
        }
    ],
    "name" : "Morris Park Bake Shop",
    "restaurant_id" : "30075445"
}
```
MongoDB'yi tanımak için örnek veri setini kullanmaya devam edebilir veya ```db.cafe.drop()``` yöntemiyle silebilirsiniz:

```
db.cafe.drop()
```
Son olarak, çıkış komutuyla kabuktan çıkın:
```
exit
```
```
Output
Bye
```
