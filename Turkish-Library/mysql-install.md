#### Tanıtım
MySQL, yaygın olarak popüler LEMP (Linux, Nginx, MySQL/MariaDB, PHP/Python/Perl) yığınının bir parçası olarak kurulan açık kaynaklı bir veritabanı yönetim sistemidir. Verilerini yönetmek için ilişkisel bir veritabanı ve SQL (Yapılandırılmış Sorgu Dili) kullanır.

CentOS 7, orijinal MySQL geliştiricileri tarafından yönetilen ve MySQL'in yerini alacak şekilde tasarlanmış bir MySQL çatalı olan MariaDB'yi tercih ediyor. CentOS 7'de yum install mysql çalıştırırsanız, MySQL yerine MariaDB kurulur. MySQL ve MariaDB'yi merak ediyorsanız, MariaDB genellikle MySQL yerine sorunsuz çalışır, bu nedenle MySQL için belirli bir kullanım durumunuz yoksa, MariaDB'yi Centos 7'ye Nasıl Kurulur kılavuzuna bakın.



##### Adım 1 — MySQL'i Yükleme
Girişte belirtildiği gibi, [MySQL'i kurmak için Yum komutu](https://dev.mysql.com/downloads/repo/yum/) aslında MariaDB'yi kurar. MySQL'i kurmak için MySQL için paketler sağlayan MySQL topluluğu Yum Deposunu ziyaret etmemiz gerekecek.

Bir web tarayıcısında şu adresi ziyaret edin:
```
https://dev.mysql.com/downloads/repo/yum/
```
Belirgin İndirme bağlantılarının doğrudan dosyalara yönlendirmediğini unutmayın. Bunun yerine, oturum açmaya veya bir hesaba kaydolmaya davet edildiğiniz sonraki bir sayfaya yönlendirilirler. Bir hesap oluşturmak istemiyorsanız, ```Hayır teşekkürler, sadece indirmemi başlat``` metnini bulabilir, ardından sağ tıklayıp bağlantı konumunu kopyalayabilir veya aşağıdaki komutlarda sürüm numarasını düzenleyebilirsiniz.

İstediğiniz sürümü bulun ve aşağıdaki bağlantıda gerektiği gibi güncelleyin:


```
curl -sSLO https://dev.mysql.com/get/mysql80-community-release-el7-5.noarch.rpm
```
rpm dosyası kaydedildikten sonra, md5sum çalıştırarak ve sitede listelenen karşılık gelen MD5 değeriyle karşılaştırarak indirmenin bütünlüğünü doğrulayacağız:
```
md5sum mysql80-community-release-el7-5.noarch.rpm
```
```
Output
e2bd920ba15cd3d651c1547661c60c7c  mysql80-community-release-el7-5.noarch.rpm
```
Bu çıktıyı sitedeki uygun MD5 değeriyle karşılaştırın:

Dosyanın bozulmadığını veya değiştirilmediğini doğruladığımıza göre, paketi yükleyeceğiz:
```
sudo rpm -ivh mysql57-community-release-el7-9.noarch.rpm
```
Bu, iki yeni MySQL yum deposu ekler ve artık bunları MySQL sunucusunu kurmak için kullanabiliriz:
```
sudo yum install mysql-server
```
Devam etmek istediğinizi onaylamak için y düğmesine basın. Paketi yeni eklediğimiz için GPG anahtarını da kabul etmemiz istenecek. İndirmek ve kurulumu tamamlamak için y tuşuna basın.

##### 2. Adım — MySQL'i Başlatma
Daemon'u aşağıdaki komutla başlatacağız:
```
sudo systemctl start mysqld
```
systemctl tüm hizmet yönetimi komutlarının sonucunu göstermez, bu nedenle başarılı olduğumuzdan emin olmak için aşağıdaki komutu kullanacağız:
```
sudo systemctl status mysqld
```
MySQL başarıyla başlatıldıysa, çıktı Active: active (çalışıyor) içermeli ve son satır şöyle görünmelidir:
```
Dec 01 19:02:20 centos-512mb-sfo2-02 systemd[1]: Started MySQL Server.
```
Not: MySQL, kurulduğunda açılışta başlayacak şekilde otomatik olarak etkinleştirilir. Bu varsayılan davranışı sudo systemctl disable mysqld ile değiştirebilirsiniz.

Kurulum işlemi sırasında, MySQL root kullanıcısı için geçici bir şifre oluşturulur. Bu komutla mysqld.log içinde bulun:
```
sudo grep 'temporary password' /var/log/mysqld.log
```
```
Output
2022-01-24T19:54:46.313728Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: mqRfBU_3Xk>r
```
Kurulumu güvenli hale getirmek için bir sonraki adımda ihtiyaç duyacağınız ve nerede değiştirmek zorunda kalacağınız şifreyi not edin. Varsayılan parola ilkesi, en az bir büyük harf, bir küçük harf, bir sayı ve bir özel karakterden oluşan 12 karakter gerektirir.

##### 3. Adım — MySQL'i Yapılandırma
MySQL, uzak kök oturum açma ve örnek kullanıcılar gibi şeyler için daha az güvenli varsayılan seçeneklerden bazılarını değiştirmek için bir güvenlik komut dosyası içerir.

Güvenlik komut dosyasını çalıştırmak için bu komutu kullanın.
```
sudo mysql_secure_installation
```
Bu, sizden varsayılan kök şifresini isteyecektir. Girdiğiniz anda, değiştirmeniz istenecektir.
```
Output
The existing password for the user account root has expired. Please set a new password.

New password:
```
En az bir büyük harf, bir küçük harf, bir sayı ve bir özel karakter içeren 12 karakterlik yeni bir şifre girin. İstendiğinde tekrar girin.

Yeni şifrenizin gücü hakkında geri bildirim alacaksınız ve ardından hemen tekrar değiştirmeniz istenecek. Az önce yaptığınız için, güvenle Hayır diyebilirsiniz:
```
Output
Estimated strength of the password: 100
Change the password for root ? (Press y|Y for Yes, any other key for No) :
```
Şifre değiştirme istemini tekrar reddettikten sonra, anonim kullanıcıları kaldırmak, uzaktan root girişine izin vermemek, test veritabanını kaldırmak ve ona erişim sağlamak ve ayrıcalık tablolarını yeniden yüklemek için ```Y```'ye ve ardından sonraki tüm sorulara ```ENTER```'a basacağız. .

Kurulumu güvenli hale getirdiğimize göre şimdi test edelim.

##### Adım 4 — MySQL'i Test Etme
Yönetim komutlarını çalıştırmanıza izin veren bir istemci olan mysqladmin aracına bağlanarak kurulumumuzu doğrulayabilir ve kurulum hakkında bilgi alabiliriz. MySQL'e ```root (-u root)``` olarak bağlanmak, bir parola istemek ```(-p)``` ve sürümü döndürmek için aşağıdaki komutu kullanın.
```
mysqladmin -u root -p version
```
Şuna benzer bir çıktı görmelisiniz:
```
Output
mysqladmin  Ver 8.0.28 for Linux on x86_64 (MySQL Community Server - GPL)
Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Server version		8.0.28
Protocol version	10
Connection		Localhost via UNIX socket
UNIX socket		/var/lib/mysql/mysql.sock
Uptime:			3 min 2 sec

Threads: 2  Questions: 14  Slow queries: 0  Opens: 133  Flush tables: 3  Open tables: 49  Queries per second avg: 0.076
```
Bu, kurulumunuzun başarılı olduğunu gösterir.