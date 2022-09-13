#### Tanıtım
Yeni bir Linux sunucusu kullanmaya başladığınızda, genellikle yapmanız gereken ilk şeylerden biri kullanıcı eklemek ve kaldırmaktır. Bu kılavuzda, bir CentOS 8 sunucusunda kullanıcı hesaplarının nasıl oluşturulacağını, sudo ayrıcalıklarının nasıl atanacağını ve kullanıcıların nasıl silineceğini ele alacağız.


##### Kullanıcı Ekleme
Bu eğitim boyunca kullanıcı adı ile çalışacağız. Lütfen seçtiğiniz kullanıcı adı ile değiştirin.

Yazarak yeni bir kullanıcı ekleyebilirsiniz:
```
sudo adduser Username
```
Ardından, oturum açabilmeleri için kullanıcınıza bir parola vermeniz gerekir. Bunu yapmak için passwd komutunu kullanın:
```
sudo passwd Username
```
Onaylamak için parolayı iki kez girmeniz istenecektir. Artık yeni kullanıcınız kuruldu ve kullanıma hazır!

Not: SSH sunucunuz parola tabanlı kimlik doğrulamaya izin vermiyorsa, henüz yeni kullanıcı adınızla bağlantı kuramazsınız. Yeni kullanıcı için anahtar tabanlı SSH kimlik doğrulamasının ayarlanmasıyla ilgili ayrıntılar, CentOS 8 ile İlk Sunucu Kurulumunun 5. adımında bulunabilir.

##### Bir Kullanıcıya Sudo Ayrıcalıkları Verme
Yeni kullanıcınızın root (yönetici) ayrıcalıklarına sahip komutları yürütme becerisine sahip olması gerekiyorsa, onlara sudo'ya erişim izni vermeniz gerekir.

Bunu, kullanıcıyı tekerlek grubuna ekleyerek yapabiliriz (bu, varsayılan olarak tüm üyelerine sudo erişimi sağlar).

Kullanıcınızı tekerlek grubuna eklemek için usermod komutunu kullanın:
```
sudo usermod -aG wheel Username
```
Nşimdi yeni kullanıcınız yönetici ayrıcalıklarıyla komutları yürütebilir. Bunu yapmak için, yönetici olarak yürütmek istediğiniz komutun önüne sudo ekleyin:
```
sudo some_command
```
Kullanıcı hesabınızın şifresini girmeniz istenecektir (kök şifresini değil). Doğru şifre gönderildiğinde, girdiğiniz komut kök ayrıcalıklarıyla yürütülecektir.

Sudo Ayrıcalıklarına Sahip Kullanıcıları Yönetme
usermod ile bir gruba kullanıcı ekleyip kaldırabilirsiniz, ancak komutun hangi kullanıcıların bir grubun üyesi olduğunu gösterme yolu yoktur.

Hangi kullanıcıların tekerlek grubunun parçası olduğunu (ve dolayısıyla sudo ayrıcalıklarına sahip olduğunu) görmek için lid komutunu kullanabilirsiniz. kapak normalde bir kullanıcının hangi gruplara ait olduğunu göstermek için kullanılır, ancak -g bayrağıyla bunu tersine çevirebilir ve hangi kullanıcıların bir gruba ait olduğunu gösterebilirsiniz:
```
sudo lid -g wheel
```
```
Output
 centos(uid=1000)
 Username(uid=1001)
 ```
Çıktı size grupla ilişkili kullanıcı adlarını ve UID'leri gösterecektir. Bu, önceki komutlarınızın başarılı olduğunu ve kullanıcının ihtiyaç duyduğu ayrıcalıklara sahip olduğunu doğrulamanın iyi bir yoludur.

##### Kullanıcıları Silme

Artık ihtiyacınız olmayan bir kullanıcı hesabınız varsa, onu silmek en iyisidir.

Kullanıcıyı, dosyalarını silmeden silmek için userdel komutunu kullanın:
```
sudo userdel Username
```
Kullanıcının ana dizinini hesabıyla birlikte silmek istiyorsanız, userdel'e -r bayrağını ekleyin:
```
sudo userdel -r Username
```
Her iki komutla da kullanıcı, varsa tekerlek grubu da dahil olmak üzere eklendikleri tüm gruplardan otomatik olarak kaldırılacaktır. Daha sonra aynı ada sahip başka bir kullanıcı eklerseniz, sudo erişimi elde etmek için tekrar tekerlek grubuna eklenmeleri gerekir.