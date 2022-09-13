## LinuxBrew'u Yükleme
LinuxBrew'u kurmak, basitçe LinuxBrew Deposunu klonlamaktan ibarettir.

##### Adım 1 - LinuxBrew'u Klonlayın
İşleri düzenli tutmak için LinuxBrew'u kullanıcının ana dizinindeki gizli bir dizine kopyalayın:
```
$ git clone https://github.com/Homebrew/linuxbrew.git ~/.linuxbrew
```
Ancak başka herhangi bir dizin de aynı şekilde çalışır.

##### 2. Adım - Ortam değişkenlerini güncelleyin
Sonraki adım, kullanıcının ortam değişkenlerine LinuxBrew eklemektir.

Kullanıcının ~/.bashrc dosyasının sonuna aşağıdaki satırları ekleyin:
```
# Until LinuxBrew is fixed, the following is required.
# See: https://github.com/Homebrew/linuxbrew/issues/47
export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:/usr/local/lib64/pkgconfig:/usr/lib64/pkgconfig:/usr/lib/pkgconfig:/usr/lib/x86_64-linux-gnu/pkgconfig:/usr/lib64/pkgconfig:/usr/share/pkgconfig:$PKG_CONFIG_PATH
## Setup linux brew
export LINUXBREWHOME=$HOME/.linuxbrew
export PATH=$LINUXBREWHOME/bin:$PATH
export MANPATH=$LINUXBREWHOME/man:$MANPATH
export PKG_CONFIG_PATH=$LINUXBREWHOME/lib64/pkgconfig:$LINUXBREWHOME/lib/pkgconfig:$PKG_CONFIG_PATH
export LD_LIBRARY_PATH=$LINUXBREWHOME/lib64:$LINUXBREWHOME/lib:$LD_LIBRARY_PATH
```
NOT: LinuxBrew'u farklı bir dizine kurduysanız, yukarıdaki LINUXBREWHOME'daki yolu değiştirin.

##### 3. Adım - Kurulumu test edin
Bu değişikliklerin geçerli olmasını sağlamak için oturumu kapatın ve tekrar oturum açın. Kabuk daha sonra bu yeni ayarları kullanmalıdır.

Bu yeni ayarları test etmek için şunu deneyin:
```
$ which brew
/home/ubuntu/.linuxbrew/bin/brew
$ echo $PKG_CONFIG_PATH
/home/ubuntu/.linuxbrew/lib64/pkgconfig:/home/ubuntu/.linuxbrew/lib/pkgconfig:/usr/local/lib/pkgconfig:/usr/local/lib64/pkgconfig:/usr/lib64/pkgconfig:/usr/lib/pkgconfig:/usr/lib/x86_64-linux-gnu/pkgconfig:/usr/lib64/pkgconfig:/usr/share/pkgconfig:
```
Paketleri LinuxBrew ile Kurmak
Hangi paketler mevcut?
Kullanılabilir tüm paketlerin listesini görmek için brew search yazın (mevcut LinuxBrew kurulumunun bildiği tüm paketler - depo ekleme hakkında aşağıya bakın).

WORD içeren tüm paketleri (HomeBrew jargonunda Formüller olarak adlandırılır) görmek için brew search WORD yazın. Örnek vermek:
```
$ brew search xml
blahtexml       libnxml   libxml2     xml-coreutils   xml2        xmlrpc-c
html-xml-utils  libwbxml  libxmlsec1  xml-security-c  xmlcatmgr   xmlsh
libmxml         libxml++  tinyxml     xml-tooling-c   xmlformat   xmlstarlet
```
###### Bir paket kurun
Bir paketi kurmak için brew install PACKAGE'i çalıştırın.

Örnek, jq - JSON işlemci kurulumu:
```
$ brew install jq
==> Downloading http://stedolan.github.io/jq/download/source/jq-1.3.tar.gz
==> ./configure
==> make
/home/ubuntu/.linuxbrew/Cellar/jq/1.3: 7 files, 256K, built in 10 seconds
$ which jq
/home/ubuntu/.linuxbrew/bin/jq
$ jq --version
jq version 1.3
```
LinuxBrew'in kullanışlılığı açıktır: Ubuntu'nun en son depolarda jq'si olsa da, sürümü eskidir (1.2). Debian Stable ve Testing'in hiç jq paketi yok. LinuxBrew'un sürümü en yeni sürümdür (1.3). Ek olarak, LinuxBrew, programı sistemin varsayılan konumuyla çakışmayacak bir yola kurar.

##### Mevcut HomeBrew Depolarını Ekleme
HomeBrew/LinuxBrew depolarına TAPS adı verilir. Bunlar basitçe Ruby betikleri ('Formüller') içeren GitHub depolarıdır. HomeBrew Githab Kullanıcısının birkaç ortak deposu vardır.

Örnek: homebrew-science deposunu (birçok faydalı açık kaynaklı bilimsel programı içerir) ve HomeBrew-Games deposunu eklemek:
```
$ brew tap homebrew/science
Cloning into '/home/ubuntu/.linuxbrew/Library/Taps/homebrew-science'...
Tapped 237 formula
$ brew tap homebrew/games
Cloning into '/home/ubuntu/.linuxbrew/Library/Taps/homebrew-games'...
Tapped 57 formula
```
Kullanılabilir muslukları listeleyin:
```
$ brew tap
homebrew/science
homebrew/games
```
Bu depolardan herhangi bir paketi yükleyin:
```
$ brew install gnu-go
==> Downloading http://ftpmirror.gnu.org/gnugo/gnugo-3.8.tar.gz
#################################################################
==> ./configure --prefix=/home/ubuntu/.linuxbrew/Cellar/gnu-go/3.8 --with-readline=/usr/lib
==> make install
/home/ubuntu/.linuxbrew/Cellar/gnu-go/3.8: 9 files, 7.0M, built in 60 seconds
```
##### brew ve Paketleri Güncelleme
Formüllerdeki herhangi bir güncellemeyi indirmek için şunu çalıştırın:
```
$ brew update
```
Paketleri yükseltmek için (güncellemeler varsa), şunu çalıştırın:
```
$ brew upgrade PACKAGE
```
##### Özel/Özel TAP'ler (Depolar) Oluşturma
HomeBrew TAP/Repository, yerel dosyalarda veya GitHub depolarında depolanan bir Formüller – Ruby komut dosyaları koleksiyonudur.

###### Yerel dosyalardaki formüller
Yerel bir dosyadan formül yüklemek için şunu çalıştırın:
```
$ brew install /full/path/to/file.rb
```
Bu, yeni bir formül oluştururken (ve hata ayıklarken) kullanışlıdır.

###### GitHub depolarındaki formüller
Github'da özel bir TAP deposu oluşturmak için, yeni bir GitHub deposu oluşturun (kullanıcınızın github hesabında) ve onu homebrew-NAME olarak adlandırın. HomeBrew/LinuxBrew musluğu olarak çalışmak için 'homebrew-' ile başlamalıdır. İSİM, istediğiniz herhangi bir isim olabilir.

Örnek vermek:

GitHub kullanıcısı agordon, gordon adında bir HomeBrew deposuna sahiptir, tam URL: https://github.com/agordon/homebrew-gordon.

Bu depoyu kullanmak için (“dokunun”):
```
$ brew tap agordon/gordon
Cloning into '/home/ubuntu/.linuxbrew/Library/Taps/agordon-gordon'...
Warning: Could not tap agordon/gordon/libestr over Homebrew/homebrew/libestr
Warning: Could not tap agordon/gordon/coreutils over Homebrew/homebrew/coreutils
Tapped 12 formula
```
###### NOTLAR

brew tap, agordon kullanıcı adını ve gordon depo son ekini ("homebrew-gordon" soneki) kullandı ve erişmek için github URL'sini çıkardı.
Özel depolardaki formüller, resmi HomeBrew depolarındaki formüllerle çakışabilir. Bu gayet normal. Bu tür paketlerin nasıl kurulacağı hakkında aşağıya bakın.
Çakışmayan paketleri özel depolardan yüklemek için şunu çalıştırın:
```
$ brew install libjson
```
Paketleri belirli musluklardan yüklemek için şunu çalıştırın:
```
$ brew install agordon/gordon/coreutils
```