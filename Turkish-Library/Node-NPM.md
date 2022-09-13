## CentOS AppStream Deposundan Düğüm Yükleme
Node.js, CentOS 8'in varsayılan AppStream yazılım deposundan edinilebilir. Kullanılabilir birden fazla sürüm vardır ve uygun modül akışını etkinleştirerek aralarından seçim yapabilirsiniz. İlk önce, dnf komutunu kullanarak nodejs modülü için mevcut akışları listeleyin:
```
sudo dnf module list nodejs
 ```
 ```
Output
Name                     Stream                   Profiles                                                Summary
nodejs                   10 [d]                   common [d], development, minimal, s2i                   Javascript runtime
nodejs                   12                       common, development, minimal, s2i                       Javascript runtime
```
10 ve 12 olmak üzere iki akış mevcuttur. [d], sürüm 10'un varsayılan akış olduğunu gösterir. Node.js 12'yi yüklemeyi tercih ederseniz, modül akışlarını şimdi değiştirin:
```
sudo dnf module enable nodejs:12
 ```
Kararınızı onaylamanız istenecektir. Daha sonra sürüm 12 akışı etkinleştirilecek ve kuruluma devam edebiliriz. Modül akışlarıyla çalışma hakkında daha fazla bilgi için resmi CentOS AppStream belgelerine bakın.

nodejs paketini dnf ile kurun:
```
sudo dnf install nodejs
 ```
Yine, dnf sizden yapacağı işlemleri onaylamanızı isteyecektir. Bunu yapmak için y'ye ve ardından ENTER'a basın, yazılım yüklenecektir.

Sürüm numarası için düğümü sorgulayarak yüklemenin başarılı olup olmadığını kontrol edin:
```
node --version
 ```
 ```
Output
v12.13.1
```
Bunun yerine Node.js 10'u yüklediyseniz --version çıktınız farklı olacaktır.

Not: Node.js'nin mevcut her iki sürümü de uzun vadeli destek sürümleridir, yani daha uzun garantili bakım aralığına sahiptirler. Daha fazla yaşam döngüsü bilgisi için resmi Node.js sürümleri sayfasına bakın.

nodejs paketini kurmak, npm Node Package Manager yardımcı programını da bir bağımlılık olarak kurmalıdır. Ayrıca doğru şekilde kurulduğunu doğrulayın:
```
npm --version
 ```
 ```
Output
6.12.1
```
Bu noktada, CentOS yazılım havuzlarını kullanarak Node.js ve npm'yi başarıyla yüklediniz. Sonraki bölüm, bunu yapmak için Düğüm Sürüm Yöneticisinin nasıl kullanılacağını gösterecektir.

### Modül akışlarını değiştirme
Bir sistemde kurulu olandan farklı bir akışa geçiş, iki adımlı bir işlemdir. İlk olarak, mevcut akışın artık etkinleştirilmemesi için sıfırlanması gerekiyor - ancak bu, paketlerini kurulu halde tutacaktır. İkincisi, yeni bir akışın yüklenmesi gerekiyor.

```
$ sudo dnf module reset NAME
$ sudo dnf module install NAME:STREAM
```