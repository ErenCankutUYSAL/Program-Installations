## Zabbix İzleme Aracı Nasıl Kurulur

Zabbix, ağ hizmetleri, ağ donanımı, sunucular ve uygulama için açık kaynaklı bir izleme aracıdır. Sisteminizin ve sunucularınızın durumunu izlemek ve izlemek için tasarlanmıştır. Zabbix, verileri depolamak için MySQL, PostgreSQL, SQLite ve IBM DB2 dahil olmak üzere birçok veritabanı sistemi için destek sunar. Zabbix arka ucu C ile yazılmıştır ve ön uç PHP ile yazılmıştır.

Bu eğitimde, CentOS 8 sistemine açık kaynaklı bir izleme sistemi Zabbix 4.0 LTS'nin nasıl kurulacağını ve yapılandırılacağını adım adım göstereceğiz. Bu kılavuz, Zabbix kurulumumuz için LAMP Stack kurulumu ve yapılandırması ve Zabbix yönetici web kullanıcı arayüzü için varsayılan parolanın nasıl değiştirileceği gibi bazı konuları kapsayacaktır.

##### Önkoşullar

- CentOS 8 Server
- Root privileges
- Basic knowledge about Linux CentOS commands
- What we will do:

##### Apache Httpd'yi yükleyin
- Install and Configure PHP
- Install and Configure MariaDB
- Install and Configure Zabbix 4.0 LTS
- Configure Firewalld
- Configure SELinux
- Zabbix Post-Installation
- Change Default Admin of Zabbix

### Adım 1 - Apache Httpd'yi yükleyin

Bu kılavuz için web sunucumuz için Apache/httpd kullanacağız. Zabbix sunucusu, Apache web sunucusu altında çalışacaktır.

Aşağıdaki dnf komutunu kullanarak Apache/httpd paketini kurabilirsiniz.


```
dnf install httpd
```

Kurulum tamamlandıktan sonra sistem önyüklemesine httpd servisini ekleyin ve aşağıdaki komutları kullanarak servisi başlatın.

```
systemctl enable httpd
systemctl start httpd
```

Şimdi aşağıdaki netstat komutunu kullanarak httpd hizmetini kontrol edin ve ```80``` HTTP portunun ``` LISTEN``` durumunda olduğundan emin olun.

```
netstat -plntu
```

Sonuç aşağıdadır.

Apache web sunucusunu etkinleştir

Sonuç olarak, httpd hizmeti, CentOS 8 sunucusundaki varsayılan ```80``` HTTP bağlantı noktasında çalışır ve çalışır.

### 2. Adım - PHP'yi yükleyin

Apache/httpd web sunucusunu kurduktan sonra, Zabbix kurulumu için gerekli PHP paketlerini ve uzantılarını kuracağız. PHP'yi Zabbix kurulum gereksinimleri olarak kuracağız ve yapılandıracağız.

Aşağıdaki dnf komutunu kullanarak PHP paketlerini kurun.

```
dnf install php-cli php-common php-devel php-pear php-gd php-mbstring php-mysqlnd php-xml php-bcmath
```
Kurulum tamamlandıktan sonra, vim düzenleyicisini kullanarak PHP yapılandırmasını ```/etc/php.ini``` yapılandırın.

```
vim /etc/php.ini
```
Yapılandırmanın değerlerini aşağıda gösterildiği gibi değiştirin.

```
date.timezone = Asia/Jakarta
max_execution_time = 600
max_input_time = 600
memory_limit = 256M
post_max_size = 32M
upload_max_filesize = 16M
```

Kaydet ve kapat.

Şimdi Apache/httpd hizmetini yeniden başlatın.

```
systemctl restart httpd
```
Ve CentOS 8 sunucusu üzerinde PHP kurulumu ve konfigürasyonu tamamlandı.

### 3. Adım - MariaDB'yi Kurun ve Yapılandırın
Varsayılan olarak, Zabbix kurulum için MySQL, PostgreSQL, SQLite ve Oracle veritabanı dahil olmak üzere birçok veritabanı için destek sunar. Bu kılavuz için, Zabbix kurulumumuz için veritabanı olarak MariaDB'yi kullanacağız.

Aşağıdaki dnf komutunu kullanarak mariadb-server'ı kurun.

```
dnf install mariadb-server
```
Kurulum tamamlandıktan sonra sistem önyüklemesine MariaDB servisini ekleyin ve aşağıdaki komutu kullanarak servisi başlatın.
```
systemctl enable mariadb
systemctl start mariadb
```
MariaDB sunucusu çalışıyor ve çalışıyor.

Ardından, MariaDB kök şifresini yapılandıracağız. Kök parolayı yapılandırmak için aşağıdaki ``mysql_secure_installation`` komutunu çalıştırın.
```
mysql_secure_installation
```
Kök parolanızı yazın ve Enter'a basın.

```
Set a root password? [Y/n] Y
Disallow root login remotely? [Y/n] Y
Remove test database and access to it? [Y/n] Y
Reload privilege tables now? [Y/n] Y
```
Ve MariaDB kök parolası yapılandırıldı.

MariaDB'yi yükleyin

Ardından, Zabbix kurulumu için yeni bir veritabanı ve kullanıcı oluşturacağız. ``hakase-labs@`` şifresi ile ```zabbix``` adında yeni bir veritabanı ve kullanıcı oluşturacağız.

Aşağıdaki mysql komutunu kullanarak MariaDB/mysql kabuğunda oturum açın.
```
mysql -u root -p
TYPE YOUR ROOT PASSWORD:
```
And run the following MariaDB queries below on the shell.

```
create database zabbix; 
grant all privileges on zabbix.* to zabbix@'localhost' identified by 'hakase-labs@'; 
grant all privileges on zabbix.* to zabbix@'%' identified by 'hakase-labs@'; 
flush privileges;
```
Ve sonuç olarak, Zabbix kurulumu için yeni veritabanı ve kullanıcı oluşturuldu.

### Adım 4 - Zabbix 4.0 LTS'yi Kurun ve Yapılandırın

Bu adımda Zabbix 4.0 LTS'yi kuracağız. Zabbix LTS sürümünü resmi Zabbix deposundan yükleyeceğiz.

##### - Depo Ekle ve Paketleri Yükle

Öncelikle aşağıdaki rpm komutunu kullanarak Zabbix 4.0 LTS deposunu sisteme eklememiz gerekiyor.
Firstly, we need to add the Zabbix 4.0 LTS repository to the system using the rpm command below.
```
rpm -Uvh https://repo.zabbix.com/zabbix/4.0/rhel/8/x86_64/zabbix-release-4.0-2.el8.noarch.rpm
```
Bundan sonra, sistem paketleri önbelleğini kaldırın ve sistemdeki mevcut tüm depoları kontrol edin.
```
dnf clean all
dnf repolist
```
Şimdi sonucu aşağıdaki gibi alacaksınız.

Zabbix RPM dosyasını yükleyin

Sonuç olarak, Zabbix deposu CentOS 8 sistemine eklendi.

Şimdi Zabbix Server ve Agent'ı kurmak için aşağıdaki dnf komutunu çalıştırın.
```
dnf -y install zabbix-server-mysql zabbix-web-mysql zabbix-agent
```
Zabbix kurulumunun bitmesini bekleyin.

gambar

##### - MySQL Veritabanı Şemasını İçe Aktar

Zabbix kurulumu tamamlandıktan sonra, Zabbix için MariaDB veritabanı şemasını içe aktaracağız.

``/usr/share/doc/zabbix-server-mysql`` dizinine gidin ve veritabanı şemasını çıkarın.

```
cd /usr/share/doc/zabbix-server-mysql
```
gunzip create.sql.gz
Şimdi aşağıdaki MySQL komutunu kullanarak veritabanı şemasını ```zabbix``` veritabanımıza aktarın.
```
mysql -u root -p zabbix < create.sql
```
MariaDB kök parolanızı yazın ve veritabanı şeması içe aktarıldı.

MySQL Veritabanı Şemasını İçe Aktar

##### - Zabbix Sunucusunu ve Zabbix Aracısını Yapılandırın
Zabbix-sunucu, Zabbix yazılım sisteminin merkezi sürecidir. '/etc/zabbix/zabbix_server.conf' yapılandırmasını düzenleyerek Zabbix sunucusunu yapılandıracağız.

``/etc/zabbix/zabbix_server.conf`` yapılandırma dosyasını vim düzenleyiciyi kullanarak düzenleyin.

```
vim /etc/zabbix/zabbix_server.conf
```

Veritabanı satırı yapılandırmasında, yapılandırmayı aşağıdaki gibi yazın ve ``DBPassword`` değerini kendi veritabanı şifrenizle değiştirin.
```
DBHost=localhost
DBPassword=hakase-labs@
```
Kaydet ve kapat.

Bundan sonra, zabbix sunucusunu sistem önyüklemesine ekleyin.
```
systemctl enable zabbix-server
```
Ve zabbix-sunucu yapılandırması tamamlandı.

Ardından, size zabbix-agent konfigürasyonunu göstereceğiz. Zabbix-agent, izlenecek tüm makinelere kurulmalıdır.

zabbix-agent yapılandırmasını `/etc/zabbix/zabbix_agentd.conf` vim düzenleyicisini kullanarak düzenleyin.

```
vim /etc/zabbix/zabbix_agentd.conf
```
Şimdi `Server` ve `ServerActive` değerini zabbix-server IP adresi ile aşağıdaki gibi değiştirin.
```
Server=10.5.5.50
ServerActive=10.5.5.50
```
Kaydet ve kapat.

Şimdi zabbix-agent hizmetini sistem önyüklemesine ekleyin.
```
systemctl enable zabbix-agent
```
Ve zabbix-agent konfigürasyonu tamamlandı.

### Adım 5 - Güvenlik Duvarını Yapılandırın
Güvenlik duvarı yapılandırması için HTTP, HTTPS ve Zabbix sunucusu ve aracısı tarafından kullanılan bağlantı noktası dahil yeni hizmetler ekleyeceğiz.

Aşağıdaki komutları kullanarak güvenlik duvarına HTTP, HTTP ve Zabbix bağlantı noktaları `10050-10051` ekleyin.
```
firewall-cmd --add-service={http,https} --permanent
firewall-cmd --add-port={10051/tcp,10050/tcp} --permanent
```
Bundan sonra, güvenlik duvarını yeniden yükleyin ve mevcut tüm hizmetleri ve bağlantı noktalarını kontrol edin.
```
firewall-cmd --reload
firewall-cmd --list-all
```
Ve sonucu aşağıdaki gibi göstereceksiniz.

Güvenlik Duvarını Yapılandır

Sonuç olarak, güvenlik duvarına HTTP, HTTPS ve Zabbix Bağlantı Noktaları ` 10050-10051` eklendi.

### Adım 6 - SELinux'u yapılandırın
Zabbix'i CentOS 8 üzerinde SELinux etkinleştirilmiş olarak çalıştırıyorsanız, bu bölümdeki tüm komutları yapmanız gerekir.

Aşağıdaki dnf komutunu kullanarak SELinux yardımcı programlarını kurun.

```
dnf install policycoreutils checkpolicy setroubleshoot-server
```
Kurulum tamamlandığında, yeni bir ```~/zabbix-linux``` dizini oluşturun ve içine girin.
```
mkdir -p ~/zabbix-selinux
cd ~/zabbix-selinux/
```
Şimdi vim düzenleyiciyi kullanarak yeni bir SELinux ilke modülü dosyası ```zabbix_server_add.te``` oluşturun.
```
vim zabbix_server_add.te
```

Aşağıdaki yapılandırmayı yapıştırın.

modül zabbix_server_add 1.1;
```
require {
        type zabbix_var_run_t;
        type tmp_t;
        type zabbix_t;
        class sock_file { create unlink write };
        class unix_stream_socket connectto;
        class process setrlimit;
        class capability dac_override;
}

#============= zabbix_t ==============

#!!!! This avc is allowed in the current policy
allow zabbix_t self:process setrlimit;

#!!!! This avc is allowed in the current policy
allow zabbix_t self:unix_stream_socket connectto;

#!!!! This avc is allowed in the current policy
allow zabbix_t tmp_t:sock_file { create unlink write };

#!!!! This avc is allowed in the current policy
allow zabbix_t zabbix_var_run_t:sock_file { create unlink write };

#!!!! This avc is allowed in the current policy
allow zabbix_t self:capability dac_override;
```
Kaydet ve kapat.

Şimdi aşağıdaki checkmodule komutunu kullanarak 'zabbix_server_add.te'yi politika modülüne dönüştürün.
```
checkmodule -M -m -o zabbix_server_add.mod zabbix_server_add.te
```
Şimdi semodule_package komutunu kullanarak ```zabbix_server_add.mod``` ilke modülünü derleyin.
```
semodule_package -m zabbix_server_add.mod -o zabbix_server_add.pp
```
Ardından derlenmiş politika modülünü ```zabbix_server_add.pp``` sisteme yükleyin.
```
semodule -i zabbix_server_add.pp
```
Zabbix için yerel özel politika modülü yüklendi.

SELinux'u yapılandırın

Sonraki, ek SELinux yapılandırması için. Aşağıdaki setbool komutunu çalıştırın.
```
setsebool -P httpd_can_network_connect 1
setsebool -P httpd_can_connect_zabbix 1
setsebool zabbix_can_network on
```
Ve Zabbix için SELinux konfigürasyonu tamamlandı.

### Adım 7 - Zabbix İlk Kurulumu
Öncelikle aşağıdaki komutları kullanarak zabbix-server'ı başlatın.
```
systemctl start zabbix-server
systemctl status zabbix-server
```
Şimdi zabbix-agent hizmetini başlatın.
```
systemctl start zabbix-agent
systemctl status zabbix-agent
```
Ardından httpd hizmetini yeniden başlatın.
```
systemctl restart httpd
```
Ardından web tarayıcınızı açın ve sunucu IP adresini aşağıdaki gibi yazın.
```
http://10.5.5.50/zabbix/
```
Ve Zabbix'ten hoş geldiniz mesajını alacaksınız.

Zabbix web yükleyici

```Sonraki Adım``` düğmesini tıklayın.

Şimdi, Zabbix kurulumu için tüm sistem gereksinimlerini kontrol edecek. Hata olmadığından emin olun.

Ön koşulları kontrol edin

```Sonraki Adım``` düğmesini tıklayın.

Veritabanı bilgisi için tüm veritabanı kurulumunuzu yazın.

Veritabanını Yapılandır

Ve ``Sonraki adım`` düğmesine tıklayın.

Şimdi Zabbix sunucu ayrıntıları yapılandırması geliyor. 'Ana Bilgisayar' alanına kendi sunucu IP adresinizi yazın ve adı kendi alan adınız veya ana bilgisayar adınızla değiştirin.

Zabbix sunucu detayları

Tekrar ``Sonraki adım`` düğmesine tıklayın.

Tüm bu konfigürasyonların doğru olduğundan emin olun, ardından Zabbix'i kurmak için "İleri Düğmesine" tıklayın.

kurulum öncesi özeti

Ve kurulum tamamlandığında aşağıdaki gibi bir sayfa ile karşılaşacaksınız.

kurulum başarılı

`Finish` düğmesine tıkladığınızda Zabbix giriş sayfasına yönlendirileceksiniz.

Varsayılan kullanıcı `admin` ve `password` ` zabbix` ile giriş yapın.

Giriş Yap

Ve varsayılan Zabbix panosunu alacaksınız.

Zabbix Panosu

Ve Zabbix kurulumu tamamlandı.

### 8. Adım - Varsayılan Yönetici Parolasını Değiştirin
Bu son adımda, zabbix için varsayılan yönetici şifresini değiştireceğiz.

Zabbix yönetici panosunda, sağ üstteki kullanıcı simgesine tıklayın.

Şifre değiştir

``Şifreyi Değiştir`` düğmesine tıklayın ve yeni şifrenizi yazın.

Şifreyi Onayla

Şimdi ``güncelle`` düğmesine tıklayın ve varsayılan yönetici şifresi değiştirildi.

Ve CentOS 8 sistemi üzerinde Zabbix Kurulumu ve Konfigürasyonu başarıyla tamamlandı.
