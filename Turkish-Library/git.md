## Git CentOS 7'ye Nasıl Kurulur

#### Tanıtım
Sürüm kontrolü, modern yazılım geliştirmede vazgeçilmez bir araç haline geldi. Versiyon kontrol sistemleri, yazılımlarınızı kaynak seviyesinde takip etmenizi sağlar. Dosya ve dizinlerin alternatif sürümlerini oluşturmak için değişiklikleri izleyebilir, önceki aşamalara dönebilir ve temel koddan ayrılabilirsiniz.

En popüler sürüm kontrol sistemlerinden biri git'tir. Birçok proje dosyalarını Git deposunda tutar ve GitHub, GitLab ve Bitbucket gibi siteler Git ile kod paylaşımını ve katkıda bulunmayı her zamankinden daha kolay hale getirdi.

Bu kılavuzda Git'in bir CentOS 7 sunucusuna nasıl kurulacağını göstereceğiz. Yazılımı, her biri kendi avantajları olan birkaç farklı yolla nasıl kuracağınızı ve hemen işbirliğine başlayabilmeniz için Git'i nasıl kuracağınızı ele alacağız.

##### Adım 1 — Git'i Yükleme
Git'i kurmanın en kolay yolu, CentOS'un varsayılan yazılım havuzlarındandır. Bu en hızlı yöntemdir, ancak bu şekilde yüklenen Git sürümü mevcut en yeni sürümden daha eski olabilir. En son sürüme ihtiyacınız varsa, git'i kaynaktan derlemeyi düşünün.

CentOS'un depolarında bulunan en son git paketini aramak ve yüklemek için CentOS'un yerel paket yöneticisi yum'u kullanın:
```
sudo yum install git
```
Komut hatasız tamamlanırsa, git'i indirmiş ve kurmuş olacaksınız. Doğru çalışıp çalışmadığını bir kez daha kontrol etmek için Git'in yerleşik sürüm kontrolünü çalıştırmayı deneyin:
```
git --version
```
Bu kontrol bir Git sürüm numarası oluşturduysa, şimdi Git'i kurmaya geçebilirsiniz.

##### 2. Adım — Git'i Kurma
Artık git'i yüklediğinize göre, kendinizle ilgili bazı bilgileri yapılandırmanız gerekecek, böylece doğru bilgiler ekli olarak taahhüt mesajları oluşturulacaktır. Bunu yapmak için, taahhütlerinize eklemek istediğiniz adı ve e-posta adresini sağlamak için git config komutunu kullanın:
```
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```
Bu konfigürasyonların başarıyla eklendiğini doğrulamak için, ayarlanan tüm konfigürasyon öğelerini yazarak görebiliriz:
```
git config --list
```
```
Output
user.name=Your Name
user.email=you@example.com
```
Bu yapılandırma, sizi bir hata mesajı görme ve taahhütleri gönderdikten sonra revize etme zahmetinden kurtaracaktır.