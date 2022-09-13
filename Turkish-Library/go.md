#### Tanıtım
Genellikle golang olarak anılan Go, Google tarafından geliştirilen açık kaynaklı bir programlama dilidir. Geliştirmeye minimalist bir yaklaşım getirir ve basit, güvenilir ve verimli yazılım oluşturmayı kolaylaştırır. Bu eğitim, Go 1.7'yi indirip kurmanın yanı sıra temel bir “Merhaba, Dünya!” Derleme ve yürütme konusunda size rehberlik edecektir. program, bir CentOS 7 sunucusunda.

##### Adım 1 – Go'yu İndirme
Eylül 2016 itibariyle, CentOS için varsayılan depolardaki golang paketi güncel değil. Sonuç olarak, paketi doğrudan Go web sitesinden manuel olarak indireceğiz. 64 bit mimariyle uyumlu en son sürümün bağlantısını kopyaladığınızdan emin olun.

Yazılabilir bir dizine geçerek başlayın:
```
cd /tmp
```
Tarball'ı indirmek için curl komutunu ve Git bağlantısını kullanın:
```
curl -LO https://storage.googleapis.com/golang/go1.7.linux-amd64.tar.gz
```
Tarball gerçek bir kaynaktan gelmesine rağmen, İnternet'ten indirilen öğelerin hem gerçekliğini hem de bütünlüğünü doğrulamak en iyi uygulamadır. Bu doğrulama yöntemi, indirme işlemi sırasında dosyanın kurcalanmadığını, bozulmadığını veya hasar görmediğini onaylar. -a 256 bayrağına sahip shasum komutu, benzersiz bir 256 bitlik karma üretir:
```
shasum -a 256 go1.7*.tar.gz
```
```
Output
702ad90f705365227e902b42d91dd1a40e48ca7f67a2f4b2fd052aaa4295cd95  go1.7.linux-amd64.tar.gz
```
Çıktınızdaki hash'i Go indirme sayfasındaki sağlama toplamı değeriyle karşılaştırın. Eşleşirlerse, indirmenin yasal olduğu sonucuna varmak güvenlidir.

Go indirildi ve dosyanın bütünlüğü doğrulandı ile kuruluma devam edelim.

##### 2. Adım – Go'yu Yükleme
Go kurulumu, tarball'ın /usr/local dizinine çıkarılmasından oluşur. -C bayrağıyla tar komutunun kullanılması, içeriği belirtilen bir dizine kaydeder. -x bayrağı çıkarma işlevini gerçekleştirir, -v ayrıntılı bir çıktı üretir, -z arşivi gzip sıkıştırma yardımcı programı aracılığıyla filtreler ve -f, aşağıdaki eylemleri gerçekleştirmesi için belirtilen dosya adını söyler:
```
sudo tar -C /usr/local -xvzf go1.7.linux-amd64.tar.gz
```
Not: Yayıncı Go'nun /usr/local dizinine yerleştirilmesini resmi olarak önerir. Başka bir konuma yüklemek, kullanılabilirliğini etkilemez, ancak özel yolun Go ortam değişkeni olan GOROOT'ta tanımlanması gerekir. Sonraki adım, ortam değişkenleriyle çalışmayı tartışır.

Ardından, kullanıcınızın ana dizini altında, bin, src ve pkg olmak üzere üç alt dizinle Go çalışma alanınızı oluşturun. Bin dizini, src dizinindeki insan tarafından okunabilen kaynak dosyalardan derlenen yürütülebilir programları içerecektir. Bu öğreticide pkg dizinini kullanmasak da, daha karmaşık programlar oluştururken kullanışlı olduğu için yine de kurmanızı öneririz. pkg dizini, programlar arasında paylaşılan yeniden kullanılabilir kod olan paket nesnelerini depolar.

Çalışma alanı dizini projelerimizi arayacağız, ancak buna istediğiniz herhangi bir ad verebilirsiniz. mkdir komutu için -p bayrağı uygun dizin ağacını yaratacaktır.
```
mkdir -p ~/projects/{bin,pkg,src}
```
Bu noktada, Go'nun kullanılması, komut satırında kurulum konumuna giden tam yolun belirtilmesini gerektirir. Go ile etkileşimi daha kullanıcı dostu hale getirmek için birkaç yol belirleyeceğiz.

##### Adım 3 – Gitmek İçin Yolları Ayarlama
Go'yu diğer komutlar gibi yürütmek için kurulum konumunu ``$PATH`` değişkenine eklememiz gerekir. Go bir sistem dizinine kuruldu, bu nedenle ortam değişkenini global olarak ayarlayacağız.

vi düzenleyicisini kullanarak ``/etc/profile.d`` dizininde bir path.sh betiği oluşturun:
```
sudo vi /etc/profile.d/path.sh
```
Aşağıdakileri dosyaya ekleyin, kaydedin ve çıkın:
```
/etc/profile.d/path.sh
export PATH=$PATH:/usr/local/go/bin
```
```Uyarı: Go farklı bir konuma kurulmuşsa yolu buna göre ayarlayın.```

Ek olarak, kullanıcınızın ```.bash_profile``` dosyasındaki GOPATH ve GOBIN Go ortam değişkenlerini en son oluşturulan çalışma alanını gösterecek şekilde tanımlayın. GOPATH değişkeni Go'ya kaynak dosyalarınızın konumunu söylerken, GOBIN değişkeni ona derlenmiş ikili dosyaların nerede oluşturulacağını söyler.

```.bash_profile``` dosyasını açın:
```
vi ~/.bash_profile
```
Dosyanın sonuna şunu ekleyin, kaydedin ve çıkın:
```
~/.bash_profile
. . .
export GOBIN="$HOME/projects/bin"
export GOPATH="$HOME/projects/src"
```
Uyarı: Adım 2'de belirtildiği gibi, Go /usr/local dizinine kurulmamışsa, GOROOT değişkenini de tanımlayın.
```
~/.bash_profile
. . .
export GOROOT="/path/to/go"
```
 Değişiklikleri geçerli BASH oturumunuza uygulamak için, güncellenmiş profilleri yeniden yüklemek için source komutunu kullanın:
```
source /etc/profile && source ~/.bash_profile
```
Go'nun özü yerindeyken, kısa bir program oluşturarak kurulumumuzun çalıştığını onaylayalım.

##### Adım 4 – Program Oluşturma
İlk programımızı yazmak, ortamımızın çalışmasını sağlayacak ve bize Go programlama diline aşina olma fırsatı verecektir.

Başlamak için yeni bir ```.go``` dosyası oluşturun:
```
vi ~/projects/src/hello.go
```
Aşağıdaki kod ana Go paketini kullanır, biçimlendirilmiş IO içerik bileşenini içe aktarır ve Hello, World! dizesini yazdırmak için yeni bir işlev ayarlar. Dosyaya şunları ekleyin:
```
~/projects/hello.go
package main

import "fmt"

func main() {
    fmt.Printf("Hello, World!\n")
}
```
Ardından dosyayı kaydedip çıkın.

Ardından, merhaba.go kaynak dosyasını go install komutuyla derleyin:
```
go install $GOPATH/hello.go
```
Artık programımızı çalıştırmaya hazırız:
```
$GOBIN/hello
```
Hello.go programı, Go'nun başarılı bir şekilde kurulduğunu onaylayan bir ``Hello, World!`` mesajı üretmelidir.