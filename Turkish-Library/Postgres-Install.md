## CentOS 8'de PostgreSQL Nasıl Kurulur ve Kullanılır


##### Tanıtım
İlişkisel veritabanı yönetim sistemleri, birçok web sitesinin ve uygulamanın önemli bir bileşenidir. Bilgiyi depolamak, organize etmek ve bilgiye erişmek için yapılandırılmış bir yol sağlarlar.

Postgres olarak da bilinen PostgreSQL, daha iyi SQL olarak bilinen Structured Query Language uygulamasını sağlayan ilişkisel bir veritabanı yönetim sistemidir. Hem büyük hem de küçük birçok popüler proje tarafından kullanılır, standartlara uygundur ve güvenilir işlemler ve okuma kilitleri olmadan eşzamanlılık gibi birçok gelişmiş özelliğe sahiptir.

Bu kılavuzu izleyerek, PostgreSQL'in en son sürümünü bir CentOS 8 sunucusuna kuracaksınız.

##### Önkoşullar
Bu öğreticiyi tamamlamak için CentOS 8 çalıştıran bir sunucuya ihtiyacınız olacak. Bu sunucunun yönetici ayrıcalıklarına sahip kök olmayan bir kullanıcısı ve firewalld ile yapılandırılmış bir güvenlik duvarı olmalıdır. Bunu ayarlamak için CentOS 8 için İlk Sunucu Kurulum kılavuzumuza bakın.

##### Adım 1 — PostgreSQL'i Yükleme
PostgreSQL, CentOS 8'in varsayılan AppStream yazılım deposundan edinilebilir ve yükleyebileceğiniz birden çok sürüm vardır. Yüklemek istediğiniz sürümle uyumlu olan uygun paketler ve bağımlılıklar koleksiyonunu etkinleştirerek bu sürümler arasında seçim yapabilirsiniz ve her koleksiyon bir modül akışı olarak adlandırılır.

CentOS 8'in varsayılan paket yöneticisi olan DNF'de modüller, birlikte daha büyük bir uygulama oluşturan özel RPM paketleri koleksiyonlarıdır. Bu, yükleme paketleri ve bağımlılıklarını kullanıcılar için daha sezgisel hale getirmeyi amaçlamaktadır.

dnf komutunu kullanarak postgresql modülü için mevcut akışları listeleyin:
```
dnf module list postgresql
 ```
 ```
Output
postgresql                           9.6                             client, server [d]                          PostgreSQL server and client module                         
postgresql                           10 [d]                          client, server [d]                          PostgreSQL server and client module                         
postgresql                           12                              client, server                              PostgreSQL server and client module
```
Bu çıktıda, AppStream deposundan kullanılabilen üç PostgreSQL sürümü olduğunu görebilirsiniz: "9.6", "10" ve "12". Postgres sürüm 10'u sağlayan akış, onu izleyen "[d]" ile belirtildiği gibi varsayılandır. Bu sürümü kurmak istiyorsanız, `sudo dnf install postgresql-server` komutunu çalıştırabilir ve bir sonraki adıma geçebilirsiniz. Bununla birlikte, '10' sürümü hala korunuyor olsa da, bu öğretici, bu yazının yazıldığı sırada en son sürüm olan Postgres sürüm 12'yi yükleyecektir.

PostgreSQL sürüm 12'yi kurmak için bu sürümün modül akışını etkinleştirmelisiniz. Bir modül akışını etkinleştirdiğinizde, varsayılan akışı geçersiz kılar ve etkinleştirilmiş akışla ilgili tüm paketleri sistemde kullanılabilir hale getirirsiniz. Bir sistemde aynı anda herhangi bir modülün yalnızca bir akışının etkinleştirilebileceğini unutmayın.

Postgres sürüm 12 için modül akışını etkinleştirmek için aşağıdaki komutu çalıştırın:
```
sudo dnf module enable postgresql:12
```
İstendiğinde, akışı etkinleştirmek istediğinizi onaylamak için y'ye ve ardından ENTER'a basın:
```
Output
====================================================================
 Package        Architecture  Version          Repository      Size
====================================================================
Enabling module streams:
 postgresql                   12                                   

Transaction Summary
====================================================================

Is this ok [y/N]: y
```
Sürüm 12 modül akışını etkinleştirdikten sonra, PostgreSQL 12'yi ve tüm bağımlılıklarını kurmak için postgresql-server paketini kurabilirsiniz:
```
sudo dnf install postgresql-server
 ```
Komut verildiğinde, y'ye ve ardından ENTER'a basarak kurulumu onaylayın:
```
Output
. . .
Install  4 Packages

Total download size: 16 M
Installed size: 62 M
Is this ok [y/N]: y
```
Yazılım yüklendiğine göre, PostgreSQL için yeni bir veritabanı kümesi hazırlamak için bazı başlatma adımlarını gerçekleştireceksiniz.

##### 2. Adım — Yeni Bir PostgreSQL Veritabanı Kümesi Oluşturma
Tablolar oluşturmaya ve bunları verilerle yüklemeye başlamadan önce yeni bir PostgreSQL veritabanı kümesi oluşturmanız gerekir. Veritabanı kümesi, tek bir sunucu örneği tarafından yönetilen bir veritabanları topluluğudur. Bir veritabanı kümesi oluşturmak, veritabanı verilerinin yerleştirileceği dizinlerin oluşturulmasından, paylaşılan katalog tablolarının oluşturulmasından ve "template1" ve "postgres" veritabanlarının oluşturulmasından oluşur.

"Şablon1 veritabanı", yeni veritabanları oluşturmak için kullanılan bir tür şablondur; 'template1'de saklanan her şey, hatta kendi eklediğiniz nesneler bile oluşturulduklarında yeni veritabanlarına yerleştirilecektir. "Postgres" veritabanı, kullanıcılar, yardımcı programlar ve üçüncü taraf uygulamalar tarafından kullanılmak üzere tasarlanmış varsayılan bir veritabanıdır.

Önceki adımda kurduğumuz Postgres paketi, düşük seviyeli veritabanı küme yönetimine yardımcı olan 'postgresql-setup' adlı kullanışlı bir komut dosyası ile birlikte gelir. Bir veritabanı kümesi oluşturmak için komut dosyasını "sudo" ve "--initdb" seçeneğiyle çalıştırın:
```
sudo postgresql-setup --initdb
 ```
Aşağıdaki çıktıyı göreceksiniz:
```
Output
 * Initializing database in '/var/lib/pgsql/data'
 * Initialized, logs are in /var/lib/pgsql/initdb_postgresql.log
 ```
Şimdi `systemctl` kullanarak PostgreSQL hizmetini başlatın:
```
sudo systemctl start postgresql
 ```
Ardından, hizmetin sunucu her önyüklendiğinde başlamasını sağlamak için systemctl'yi bir kez daha kullanın:
```
sudo systemctl enable postgresql
 ```
Bu aşağıdaki çıktıyı verecektir
```
Output
Created symlink /etc/systemd/system/multi-user.target.wants/postgresql.service → /usr/lib/systemd/system/postgresql.service.
```
Artık PostgreSQL çalışır durumda olduğuna göre, Postgres'in nasıl çalıştığını ve geçmişte kullanmış olabileceğiniz benzer veritabanı yönetim sistemlerinden nasıl farklı olduğunu öğrenmek için rollerin üzerinden geçeceğiz.

##### 3. Adım — PostgreSQL Rollerini ve Veritabanlarını Kullanma
PostgreSQL, istemci kimlik doğrulamasını ve yetkilendirmeyi işlemek için roller adı verilen bir kavram kullanır. Bunlar bazı yönlerden normal Unix tarzı hesaplara benzer, ancak Postgres kullanıcılar ve gruplar arasında ayrım yapmaz ve bunun yerine daha esnek terim rolünü tercih eder.

Kurulumun ardından Postgres, kimlik doğrulamasını kullanacak şekilde ayarlanır, yani "Postgres" rollerini eşleşen bir Unix/Linux sistem hesabıyla ilişkilendirir. 'Postgres' içinde bir rol varsa, aynı ada sahip bir Unix/Linux kullanıcı adı bu rol olarak oturum açabilir.

Yükleme prosedürü, varsayılan postgres rolüyle ilişkili postgres adlı bir kullanıcı hesabı oluşturdu. PostgreSQL'i kullanmak için o hesaba giriş yapabilirsiniz.

PostgreSQL istemine erişmek için bu hesabı kullanmanın birkaç yolu vardır.

Postgres Hesabına Geçiş
Aşağıdakileri yazarak sunucunuzdaki postgres hesabına geçin:
```
sudo -i -u postgres
 ```
Artık şunu yazarak bir Postgres istemine hemen erişebilirsiniz:
```
psql
 ```
Bu, PostgreSQL isteminde oturum açmanızı sağlar ve buradan veritabanı yönetim sistemiyle hemen etkileşime geçmekte özgürsünüz.

Aşağıdakileri yazarak PostgreSQL isteminden çıkın:
 ```
\q
  ```
Bu sizi "postgres" hesabının Linux komut istemine geri getirecektir. Şimdi aşağıdakilerle birlikte orijinal hesabınıza dönün:
 ```
çıkış
  ```
##### Hesap Değiştirmeden Postgres İstemine Erişme
Komutları, doğrudan "sudo" kullanarak "postgres" hesabıyla da çalıştırabilirsiniz.

Örneğin, önceki örnekte, önce postgres kullanıcısına geçerek ve ardından Postgres istemini açmak için psql çalıştırarak 'Postgres' istemine erişmeniz talimatı verildi. Alternatif olarak, postgres kullanıcısı olarak 'sudo' ile 'psql' tek komutunu çalıştırarak bunu tek adımda şöyle yapabilirsiniz:
 ```
sudo -u postgres psql
  ```
Bu, aracı bash kabuğu olmadan doğrudan Postgres'e giriş yapmanızı sağlar.

Yine şunu yazarak etkileşimli Postgres oturumundan çıkabilirsiniz:
 ```
\Q
  ```
Bu adımda, psql istemine ulaşmak için postgres hesabını kullandınız. Ancak birçok kullanım durumu, birden fazla Postgres rolü gerektirir. Yeni rollerin nasıl yapılandırılacağını öğrenmek için okumaya devam edin.

##### 4. Adım — Yeni Bir Rol Oluşturma
Şu anda, veritabanı içinde yapılandırılmış postgres rolüne sahipsiniz. `createrole` komutu ile komut satırından yeni roller oluşturabilirsiniz. `--interactive` bayrağı sizden yeni rolün adını ve ayrıca süper kullanıcı izinlerine sahip olup olmayacağını soracaktır.

`postgres` hesabıyla giriş yaptıysanız, şunu yazarak yeni bir kullanıcı oluşturabilirsiniz:
 ```
createuser --interactive
  ```
Bunun yerine, normal hesabınızdan geçiş yapmadan her komut için `sudo` kullanmayı tercih ederseniz, şunu yazın:
 ```
sudo -u postgres createuser --interactive
  ```
Komut dosyası size bazı seçenekler soracak ve yanıtlarınıza göre, belirlediğiniz özelliklere göre bir kullanıcı oluşturmak için gerekli Postgres komutlarını yürütecektir. Bu öğretici için test adında bir rol oluşturun ve istendiğinde y girerek ona süper kullanıcı ayrıcalıkları verin:
 ```
Output
Enter name of role to add: test
Shall the new role be a superuser? (y/n) y
 ```
Bazı ek bayrakları geçerek daha fazla kontrol elde edebilirsiniz. 'createuser' için kılavuz sayfasına bakarak seçeneklere göz atın:
 ```
man createuser
  ```
Postgres kurulumunuzun artık yeni bir rolü var, ancak henüz herhangi bir veri tabanı eklemediniz. Bir sonraki bölüm bu süreci açıklamaktadır.

##### Adım 5 — Yeni Bir Veritabanı Oluşturma
Postgres kimlik doğrulama sisteminin varsayılan olarak yaptığı bir başka varsayım, oturum açmak için kullanılan herhangi bir rol için, bu rolün erişebileceği aynı ada sahip bir veritabanına sahip olacağıdır.

Bu, son bölümde oluşturduğunuz kullanıcıya 'test' adı verilirse, bu rolün varsayılan olarak test olarak da adlandırılan bir veritabanına bağlanmaya çalışacağı anlamına gelir. `createdb` komutu ile böyle bir veritabanı oluşturabilirsiniz.

Postgres hesabı olarak giriş yaptıysanız, şöyle bir şey yazarsınız:
 ```
createdb test
  ```
Bunun yerine, normal hesabınızdan geçiş yapmadan her komut için "sudo" kullanmayı tercih ederseniz, şunu yazarsınız:
 ```
sudo -u postgres createdb test
  ```
Bu esneklik, gerektiğinde veritabanları oluşturmak için birden çok yol sağlar.

Artık yeni bir veritabanı oluşturduğunuza göre, yeni rolünüzle oturum açacaksınız.

##### 6. Adım — Yeni Rol ile Postgres İstemi Açma
Kimlik tabanlı kimlik doğrulama ile oturum açmak için Postgres rolünüz ve veritabanınızla aynı ada sahip bir Linux kullanıcısına ihtiyacınız olacak.

Uygun bir Linux kullanıcınız yoksa, 'adduser' komutuyla bir tane oluşturabilirsiniz. Bunu "sudo" ayrıcalıklarıyla root olmayan hesabınızdan yapmanız gerekecek (yani, postgres kullanıcısı olarak oturum açmamışsınız):
 ```
sudo adduser test
  ```
Bu yeni hesap kullanılabilir olduğunda, geçiş yapabilir ve ardından önce şunu yazarak veritabanına bağlanabilirsiniz:
 ```
sudo -i -u test
psql
  ```
Or, you can do this inline:
 ```
sudo -u test psql
  ```
This command will log you in automatically.

If you want your user to connect to a different database, you can do so by including the `-d` flag and specifying the database, like this:
 ```
psql -d postgres
  ```
Once logged in, you can check your current connection information by typing:
 ```
\conninfo
  ```
This will show the following output:
 ```
Output
You are connected to database "test" as user "test" via socket in "/var/run/postgresql" at port "5432". 
```
Bu, varsayılan olmayan veritabanlarına veya varsayılan olmayan kullanıcılarla bağlanıyorsanız kullanışlıdır.

Veritabanınıza bağlandıktan sonra artık tablolar oluşturmayı ve silmeyi deneyebilirsiniz.

##### Adım 7 — Tablolar Oluşturma ve Silme
Artık PostgreSQL veritabanı sistemine nasıl bağlanacağınızı bildiğinize göre, bazı temel Postgres yönetim görevlerini öğrenebilirsiniz.

İlk olarak, bazı verileri depolamak için bir tablo oluşturun. Örnek olarak, bazı oyun alanı ekipmanlarını açıklayan bir tablo yapacaksınız.

Bu komutun temel sözdizimi aşağıdaki gibidir:
 ```
CREATE TABLE table_name (
    column_name1 col_type (field_length) column_constraints,
    column_name2 col_type (field_length),
    column_name3 col_type (field_length)
);
 ```
Bu komutlar tabloya bir ad verir ve ardından sütunları, sütun türünü ve alan verilerinin maksimum uzunluğunu tanımlar. Ayrıca isteğe bağlı olarak her sütun için tablo kısıtlamaları ekleyebilirsiniz.

Gösteri amacıyla aşağıdaki gibi basit bir tablo oluşturun:
 ```
CREATE TABLE playground (
    equip_id serial PRIMARY KEY,
    type varchar (50) NOT NULL,
    color varchar (25) NOT NULL,
    location varchar(25) check (location in ('north', 'south', 'west', 'east', 'northeast', 'southeast', 'southwest', 'northwest')),
    install_date date
);
  ```
Bu komut, oyun alanı ekipmanının envanterini çıkaran bir tablo oluşturacaktır. ```Seri``` tipte bir ekipman kimliği ile başlar. Bu veri türü, otomatik artan bir tamsayıdır. Ayrıca bu sütuna ```PRIMARY KEY``` kısıtlamasını da verdiniz; bu, değerlerin benzersiz olması ve boş olmaması gerektiği anlamına gelir.

Sütunlardan ikisi (```equip_id``` ve ```install_date```) için komut bir alan uzunluğu belirtmez. Bunun nedeni, uzunluk tür tarafından ima edildiğinden, bazı sütun türlerinin belirli bir uzunluk gerektirmemesidir.

Sonraki iki satır, sırasıyla donanım "tipi" ve "renk" için her biri boş olamayacak sütunlar oluşturur. Bunlardan sonraki satır, bir "konum" sütunu ve değerin olası sekiz değerden biri olmasını gerektiren bir kısıtlama oluşturur. Son satır, ekipmanı kurduğunuz tarihi kaydeden bir tarih sütunu oluşturur.

Yeni tablonuzu yazarak görebilirsiniz:
  ```
\d
   ```
Bu, aşağıdaki çıktıyı gösterecektir:
 ```
Output
                  List of relations
 Schema |          Name           |   Type   | Owner 
--------+-------------------------+----------+-------
 public | playground              | table    | test
 public | playground_equip_id_seq | sequence | test
(2 rows)
 ```
Oyun alanı masanız burada, ancak "sıra" türünde "playground_equip_id_seq" adlı bir şey de var. Bu, 'equip_id' sütununuza verdiğiniz 'seri' türünün bir temsilidir. Bu, sıradaki bir sonraki sayının kaydını tutar ve bu türdeki sütunlar için otomatik olarak oluşturulur.

Yalnızca tabloyu sıra olmadan görmek istiyorsanız şunu yazabilirsiniz:
  ```
\dt
   ```
Bu, aşağıdakileri verecektir:
 ```
Output
          List of relations
 Schema |    Name    | Type  | Owner 
--------+------------+-------+-------
 public | playground | table | test
(1 row)
 ```
Bu adımda örnek bir tablo oluşturdunuz. Bir sonraki adımda, o tablodaki girdileri eklemeyi, sorgulamayı ve silmeyi deneyeceksiniz.

##### 8. Adım — Tabloya Veri Ekleme, Sorgulama ve Silme
Artık bir tablonuz olduğuna göre, içine bazı veriler ekleyebilirsiniz.

Örnek olarak, eklemek istediğiniz tabloyu çağırarak, sütunları adlandırarak ve ardından her sütun için aşağıdaki gibi veri sağlayarak bir slayt ve salıncak ekleyin:
 ```
INSERT INTO playground (type, color, location, install_date) VALUES ('slide', 'blue', 'south', '2017-04-28');
INSERT INTO playground (type, color, location, install_date) VALUES ('swing', 'yellow', 'northwest', '2018-08-16');
  ```
Birkaç yaygın takılmadan kaçınmak için verileri girerken dikkatli olmalısınız. Birincisi, sütun adlarını tırnak içine almayın, ancak girdiğiniz sütun değerleri için tırnak işaretleri gerekir.

Unutulmaması gereken bir diğer şey de, donat_id sütunu için bir değer girmemenizdir. Bunun nedeni, tabloda yeni bir satır oluşturulduğunda otomatik olarak oluşturulmasıdır.

Yazarak eklediğiniz bilgileri alın:
 ```
SELECT * FROM playground;
  ```
Aşağıdaki çıktıyı göreceksiniz:
 ```
Output
 equip_id | type  | color  | location  | install_date 
----------+-------+--------+-----------+--------------
        1 | slide | blue   | south     | 2017-04-28
        2 | swing | yellow | northwest | 2018-08-16
(2 rows)
 ```
Burada, `equip_id` nizin başarıyla doldurulduğunu ve diğer tüm verilerinizin doğru bir şekilde düzenlendiğini görebilirsiniz.

Oyun alanındaki slayt kırılırsa ve onu kaldırmanız gerekirse, şunu yazarak da tablonuzdan satırı kaldırabilirsiniz:
 ```
DELETE FROM playground WHERE type = 'slide';
  ```
Tabloyu tekrar sorgulayın:
 ```
SELECT * FROM playground;
  ```
Aşağıdakileri göreceksiniz:
 ```
Output
 equip_id | type  | color  | location  | install_date 
----------+-------+--------+-----------+--------------
        2 | swing | yellow | northwest | 2018-08-16
(1 row)
 ```
Slaytınızın artık tablonun bir parçası olmadığına dikkat edin.

Tablonuzdaki girdileri ekleyip sildiğinize göre, artık sütun eklemeyi ve silmeyi deneyebilirsiniz.

##### Adım 9 — Tablodan Sütun Ekleme ve Silme
Bir tablo oluşturduktan sonra, sütun eklemek veya kaldırmak için onu değiştirebilirsiniz. Her ekipman parçası için son bakım ziyaretini göstermek için şunu yazarak bir sütun ekleyin:
 ```
ALTER TABLE playground ADD last_maint date;
  ```
Tablo bilgilerinizi tekrar görüntülerseniz, yeni sütunun eklendiğini (ancak veri girilmediğini) göreceksiniz:
 ```
SELECT * FROM playground;
  ```
Aşağıdakileri göreceksiniz:
 ```
Output
 equip_id | type  | color  | location  | install_date | last_maint 
----------+-------+--------+-----------+--------------+------------
        2 | swing | yellow | northwest | 2018-08-16   | 
(1 row)
 ```
Bir sütunu silmek de aynı derecede basittir. Çalışma ekibinizin bakım geçmişini takip etmek için ayrı bir araç kullandığını fark ederseniz, şunu yazarak sütunu silebilirsiniz:
 ```
ALTER TABLE playground DROP last_maint;
  ```
Bu, `last_maint` sütununu ve içinde bulunan tüm değerleri siler, ancak diğer tüm verileri olduğu gibi bırakır.

Sütunları ekleyip sildikten sonra, son adımda mevcut verileri güncellemeyi deneyebilirsiniz.

##### Adım 10 — Tablodaki Verileri Güncelleme
Şimdiye kadar bir tabloya nasıl kayıt ekleyeceğinizi ve bunları nasıl sileceğinizi öğrendiniz, ancak bu öğretici henüz mevcut girdilerin nasıl değiştirileceğini kapsamadı.

Mevcut bir girdinin değerlerini, istediğiniz kaydı sorgulayarak ve sütunu kullanmak istediğiniz değere ayarlayarak güncelleyebilirsiniz. Swing kaydını sorgulayabilir (bu, tablonuzdaki her `salıncak` ile eşleşir) ve rengini `kırmızı` olarak değiştirebilirsiniz:
 ```
UPDATE playground SET color = 'red' WHERE type = 'swing';
  ```
Verileri tekrar sorgulayarak işlemin başarılı olduğunu doğrulayabilirsiniz:
 ```
SELECT * FROM playground;
  ```
Aşağıdakileri göreceksiniz:
 ```
Output
 equip_id | type  | color | location  | install_date 
----------+-------+-------+-----------+--------------
        2 | swing | red   | northwest | 2010-08-16
(1 row)
 ```
Gördüğünüz gibi slaytınız artık kırmızı olarak kaydedildi.