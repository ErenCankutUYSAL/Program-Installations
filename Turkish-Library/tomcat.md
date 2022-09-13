## CentOS 7'de Apache Tomcat 8 Nasıl Kurulur

##### Tanıtım
Apache Tomcat, Java uygulamalarına hizmet etmek için kullanılan bir web sunucusu ve sunucu uygulaması kapsayıcısıdır. Tomcat, Apache Software Foundation tarafından yayınlanan Java Servlet ve JavaServer Pages teknolojilerinin açık kaynaklı bir uygulamasıdır. Bu eğitim, CentOS 7 sunucunuzdaki Tomcat 8'in en son sürümünün temel kurulumunu ve bazı yapılandırmalarını kapsar.

##### Önkoşullar
Bu kılavuza başlamadan önce, sunucunuzda ayrı, root olmayan bir kullanıcı hesabınız olmalıdır. CentOS 7 için ilk sunucu kurulumunda 1-3 arasındaki adımları tamamlayarak bunu nasıl yapacağınızı öğrenebilirsiniz. Bu eğitimin geri kalanında burada oluşturulan demo kullanıcısını kullanacağız.

### Java'yı yükleyin
Tomcat, Java'nın sunucuya yüklenmesini gerektirir, böylece herhangi bir Java web uygulaması kodu çalıştırılabilir. OpenJDK 7'yi yum ile kurarak bu gereksinimi karşılayalım.

yum kullanarak OpenJDK 7 JDK'yı kurmak için şu komutu çalıştırın:
```
sudo yum install java-1.7.0-openjdk-devel
```

OpenJDK 7'yi yüklemeye devam etmek için komut isteminde y yanıtını verin.

Tomcat'i daha sonra yapılandırmamız gereken JAVA_HOME dizininin kısayolunun ```/usr/lib/jvm/jre.``` konumunda bulunabileceğini unutmayın.

Artık Java yüklendiğine göre, Tomcat hizmetini çalıştırmak için kullanılacak bir "Tomcat" kullanıcısı oluşturalım.

### Tomcat Kullanıcısı Oluştur
Güvenlik amacıyla, Tomcat ayrıcalıksız bir kullanıcı olarak çalıştırılmalıdır (yani kök değil). Tomcat hizmetini çalıştıracak yeni bir kullanıcı ve grup oluşturacağız.

İlk önce yeni bir  ``` Tomcat``` grubu oluşturun:
```
sudo groupadd tomcat
```

Ardından yeni bir ``` Tomcat``` kullanıcısı oluşturun. Bu kullanıcıyı ```/opt/tomcat``` (Tomcat'i kuracağımız yer) ana dizini ve "" kabuğu ile ```Tomcat``` grubunun bir üyesi yapacağız. ```/bin/false``` (kimse hesaba giriş yapamaz):
```
sudo useradd -M -s /bin/nologin -g tomcat -d /opt/tomcat tomcat
```

Artık ``Tomcat`` kullanıcımız kurulduğuna göre Tomcat'i indirip yükleyelim.


### Tomcat'i yükleyin
Tomcat 8'i şu anda kurmanın en kolay yolu, en son ikili sürümü indirip manuel olarak yapılandırmaktır.

##### Tomcat İkili Dosyasını İndirin

Tomcat 8'in en son sürümünü Tomcat 8 İndirilenler sayfasında bulun. Yazma sırasında en son sürüm 8.5.37'dir. İkili Dağılımlar bölümünün altında, ardından Çekirdek listesinin altındaki bağlantıyı “tar.gz” dosyasına kopyalayın.

En son ikili dağıtımı wget kullanarak ana dizinimize indirelim.

İlk olarak, ```yum``` paket yöneticisini kullanarak ```wget``` kurun:
```
sudo yum install wget
```
Ardından, ana dizininize geçin:
```
cd
```
Şimdi, ```wget``` kullanın ve Tomcat 8 arşivini indirmek için bağlantıyı şu şekilde yapıştırın (yansıtma bağlantınız muhtemelen örnekten farklı olacaktır):
```
wget https://www-eu.apache.org/dist/tomcat/tomcat-8/v8.5.37/bin/apache-tomcat-8.5.37.tar.gz
```
Tomcat'i ```/opt/tomcat``` dizinine kuracağız. Dizini oluşturun, ardından şu komutlarla arşivi çıkarın:
```
sudo mkdir /opt/tomcat
sudo tar xvf apache-tomcat-8*tar.gz -C /opt/tomcat --strip-components=1
```
Artık uygun kullanıcı izinlerini ayarlamaya hazırız.

### İzinleri Güncelle
Kurduğumuz ``` tomcat``` kullanıcısının Tomcat kurulumuna uygun erişime sahip olması gerekiyor. Bunu şimdi ayarlayacağız.

Tomcat kurulum yoluna geçin:
```
cd /opt/tomcat
```

Tüm kurulum dizini üzerinde ``` Tomcat``` grubunun sahipliğini verin:

```
sudo chgrp -R tomcat /opt/tomcat
```

Ardından, ``` Tomcat``` grubuna ```conf```` dizinine ve tüm içeriğine okuma erişimi verin ve dizinin kendisine erişimi yürütün:

```
sudo chmod -R g+r conf
sudo chmod g+x conf
```

Ardından ```Tomcat``` kullanıcısını ```webapps```, ```work```, ```temp``` ve ```logs``` dizinlerinin sahibi yapın:
```
sudo chown -R tomcat webapps/ work/ temp/ logs/
```

Artık uygun izinler ayarlandığına göre, bir Systemd birim dosyası oluşturalım.

### Systemd Birim Dosyasını Yükleyin

Tomcat'i hizmet olarak çalıştırabilmek istediğimiz için bir Tomcat Systemd birim dosyası oluşturacağız.

Bu komutu çalıştırarak birim dosyasını oluşturun ve açın:
```
sudo vi /etc/systemd/system/tomcat.service
```

Aşağıdaki komut dosyasına yapıştırın. ```CATALINA_OPTS:``` içinde belirtilen bellek ayırma ayarlarını da değiştirmek isteyebilirsiniz.
```
# Systemd unit file for tomcat
[Unit]
Description=Apache Tomcat Web Application Container
After=syslog.target network.target

[Service]
Type=forking

Environment=JAVA_HOME=/usr/lib/jvm/jre
Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_BASE=/opt/tomcat
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/bin/kill -15 $MAINPID

User=tomcat
Group=tomcat
UMask=0007
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
```
Kaydet ve çık. Bu komut dosyası, sunucuya Tomcat hizmetini belirtilen ayarlarla ```Tomcat``` kullanıcısı olarak çalıştırmasını söyler.

Şimdi Tomcat birim dosyasını yüklemek için Systemd'yi yeniden yükleyin:
```
sudo systemctl daemon-reload
```

Şimdi bu ```systemctl``` komutuyla Tomcat hizmetini başlatabilirsiniz:
```
sudo systemctl start tomcat
```
Artık bu systemctl komutuyla Tomcat hizmetini başlatabilirsiniz:
```
sudo systemctl start tomcat
```
Yazarak hizmetin başarıyla başlatıldığını kontrol edin:
```
sudo systemctl status tomcat
 ```
Tomcat hizmetini sunucu açılışında başlayacak şekilde etkinleştirmek istiyorsanız, şu komutu çalıştırın:
```
sudo systemctl enable tomcat
 ```
Tomcat henüz tam olarak kurulmadı, ancak bir web tarayıcısında alan adınıza veya IP adresinize ve ardından ```:8080``` yazarak varsayılan açılış sayfasına erişebilirsiniz:

```
Open in web browser:
http://server_IP_address:8080
```
Diğer bilgilere ek olarak varsayılan Tomcat açılış sayfasını göreceksiniz. Şimdi Tomcat kurulumuna daha derine ineceğiz.

### Tomcat Web Yönetim Arayüzünü Yapılandırın
Tomcat ile birlikte gelen yönetici web uygulamasını kullanabilmek için Tomcat sunucumuza bir giriş eklemeliyiz. Bunu ``` Tomcat-users.xml``` dosyasını düzenleyerek yapacağız:
```
sudo vi /opt/tomcat/conf/tomcat-users.xml
```
Bu dosya, dosyanın nasıl yapılandırılacağını açıklayan yorumlarla doludur. Aşağıdaki iki satır arasındaki tüm yorumları silmek isteyebilirsiniz veya örneklere atıfta bulunmak istiyorsanız onları bırakabilirsiniz:
```
<tomcat-users>
...
</tomcat-users>
 ```
``manager-gui`` ve ```admin-gui``` (Tomcat ile birlikte gelen webapps) erişimine sahip bir kullanıcı eklemek isteyeceksiniz. Aşağıdaki örneğe benzer bir kullanıcı tanımlayarak bunu yapabilirsiniz. Kullanıcı adını ve şifreyi güvenli bir şekilde değiştirdiğinizden emin olun:
```
<tomcat-users>
    <user username="admin" password="password" roles="manager-gui,admin-gui"/>
</tomcat-users>
 ```
Tomcat-users.xml dosyasını kaydedin ve çıkın.

Varsayılan olarak, Tomcat'in daha yeni sürümleri, Yönetici ve Ana Bilgisayar Yöneticisi uygulamalarına erişimi, sunucunun kendisinden gelen bağlantılarla kısıtlar. Uzak bir makineye kurulum yaptığımız için, muhtemelen bu kısıtlamayı kaldırmak veya değiştirmek isteyeceksiniz. Bunlar üzerindeki IP adresi kısıtlamalarını değiştirmek için uygun ```context.xml``` dosyalarını açın.

Yönetici uygulaması için şunu yazın:
```
sudo vi /opt/tomcat/webapps/manager/META-INF/context.xml
 ```
Host Manager uygulaması için şunu yazın:
```
sudo vi /opt/tomcat/webapps/host-manager/META-INF/context.xml
 ```
İçeride, herhangi bir yerden bağlantılara izin vermek için IP adresi kısıtlamasını yorumlayın. Alternatif olarak, yalnızca kendi IP adresinizden gelen bağlantılara erişime izin vermek istiyorsanız, genel IP adresinizi listeye ekleyebilirsiniz:
```
<Context antiResourceLocking="false" privileged="true" >
  <!--<Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />-->
</Context>
 ```
İşiniz bittiğinde dosyaları kaydedin ve kapatın.

Değişikliklerimizi yürürlüğe koymak için Tomcat hizmetini yeniden başlatın:
```
sudo systemctl restart tomcat
```














