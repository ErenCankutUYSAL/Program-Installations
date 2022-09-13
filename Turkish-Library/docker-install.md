## Docker Engine'i CentOS'a yükleyin

CentOS'ta Docker Engine'i kullanmaya başlamak için ön koşulları karşıladığınızdan emin olun, ardından Docker'ı yükleyin.

Önkoşullar
işletim sistemi gereksinimleri
Docker Engine'i yüklemek için CentOS 7 veya 8'in bakımlı bir sürümüne ihtiyacınız vardır. Arşivlenmiş sürümler desteklenmez veya test edilmez.

`centos-extras` deposu etkinleştirilmelidir. Bu depo varsayılan olarak etkindir, ancak devre dışı bıraktıysanız yeniden etkinleştirmeniz gerekir.

`overlay2` depolama sürücüsü önerilir.

Eski sürümleri kaldır
Docker'ın eski sürümlerine docker veya docker-engine adı verildi. Bunlar kuruluysa, ilişkili bağımlılıklarla birlikte bunları kaldırın.
```
 sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```
`yum` bu paketlerin hiçbirinin kurulu olmadığını bildirirse sorun olmaz.

Görüntüler, kapsayıcılar, birimler ve ağlar dahil `/var/lib/docker/` içeriği korunur. Docker Engine paketi artık "docker-ce" olarak adlandırılıyor.

##### Depoyu kullanarak yükleyin
Docker Engine'i yeni bir ana makineye ilk kez yüklemeden önce Docker deposunu ayarlamanız gerekir. Daha sonra, Docker'ı depodan yükleyebilir ve güncelleyebilirsiniz.

##### Depoyu kurun
`yum-utils` paketini ("yum-config-manager" yardımcı programını sağlayan) kurun ve kararlı depoyu kurun.
```
 sudo yum install -y yum-utils
 sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```
###### İsteğe bağlı: Gecelik veya test depolarını etkinleştirin.

Bu depolar, yukarıdaki docker.repo dosyasına dahil edilmiştir ancak varsayılan olarak devre dışı bırakılmıştır. Bunları kararlı deponun yanında etkinleştirebilirsiniz. Aşağıdaki komut, gecelik depoyu etkinleştirir.
```
 sudo yum-config-manager --enable docker-ce-nightly
```
Test kanalını etkinleştirmek için aşağıdaki komutu çalıştırın:
```
 sudo yum-config-manager --enable docker-ce-test
```
--disable bayrağıyla yum-config-manager komutunu çalıştırarak her gece veya test havuzunu devre dışı bırakabilirsiniz. Yeniden etkinleştirmek için --enable bayrağını kullanın. Aşağıdaki komut, gecelik depoyu devre dışı bırakır.
```
 sudo yum-config-manager --disable docker-ce-nightly
```
##### Docker Engine'i kurun
1. Docker Engine ve containerd'ın en son sürümünü yükleyin veya belirli bir sürümü yüklemek için sonraki adıma geçin:

```
sudo yum install docker-ce docker-ce-cli containerd.io
```
GPG anahtarını kabul etmeniz istenirse, parmak izinin `060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35` ile eşleştiğini doğrulayın ve öyleyse kabul edin.
```
Got multiple Docker repositories?

If you have multiple Docker repositories enabled, installing or updating without specifying a version in the yum install or yum update command always installs the highest possible version, which may not be appropriate for your stability needs.
```
Bu komut Docker'ı yükler, ancak Docker'ı başlatmaz. Ayrıca bir "docker" grubu oluşturur, ancak varsayılan olarak gruba herhangi bir kullanıcı eklemez.

2. Docker Engine'in belirli bir sürümünü yüklemek için depoda mevcut sürümleri listeleyin, ardından seçip yükleyin:

a. Deponuzda bulunan sürümleri listeleyin ve sıralayın. Bu örnek, sonuçları sürüm numarasına göre en yüksekten en düşüğe sıralar ve kısaltılır:
```
yum list docker-ce --showduplicates | sort -r
```
Döndürülen liste, hangi depoların etkinleştirildiğine bağlıdır ve CentOS sürümünüze özeldir (bu örnekte ".el7" son ekiyle belirtilir).

B. Belirli bir sürümü, paket adı ('docker-ce') artı sürüm dizesi (2. sütun) olan tam paket adıyla ilk iki nokta üst üste (:) ile başlayarak ilk kısa çizgiye kadar, bir tire ile ayrılmış olarak yükleyin (-). Örneğin, "docker-ce-18.09.1".
```
 sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
 ```
Bu komut Docker'ı yükler, ancak Docker'ı başlatmaz. Ayrıca bir "docker" grubu oluşturur, ancak varsayılan olarak gruba herhangi bir kullanıcı eklemez.

Docker'ı başlatın.
```
 sudo systemctl start docker
 ```
`hello-world` görüntüsünü çalıştırarak Docker Engine'in doğru şekilde kurulduğunu doğrulayın.
```
 sudo docker run hello-world
 ```
Bu komut, bir test görüntüsünü indirir ve bir kapsayıcıda çalıştırır. Konteyner çalıştığında, bir mesaj yazdırır ve çıkar.

Bu, Docker Engine'i yükler ve çalıştırır. Docker komutlarını çalıştırmak için `sudo` kullanın. Ayrıcalıklı olmayan kullanıcıların Docker komutlarını çalıştırmasına izin vermek ve diğer isteğe bağlı yapılandırma adımları için `Linux yükleme sonrası` ile devam edin.
