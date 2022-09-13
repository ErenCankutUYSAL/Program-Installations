## Chmod Komutu İle Yetkilendirme İşlemleri

```Chmod``` komutu, linux sistemleri içerisinde dosya ve klasör izinlerini ```kullanıcı, grup ve diğer kişilere``` atayabilmemizi ya da var olan izinleri değiştirebilmemizi sağlayan bir linux sistem komutudur.

Bir dosya ya da dizinin erişim hakkını ancak dosyanın ```sahibi (owner)``` ya da ```üst kullanıcı (superuser)``` değiştirebilir.

chmod komutunun kullandığı parametreleri linux terminali üzerinde ```chmod –help``` komutunu kullanarak öğrenebilirsiniz.

### Şimdi gelin ```chmod``` komutumuzun kullandığı parametrelerin ne anlama geldiklerine bakalım.

```-c, –changes =``` Ayrıntılı gibi ama yalnızca bir değişiklik yapıldığında rapor et. <br>
```-f, –silent, –quiet =``` Karşılaşılan çoğu hata mesajını gösterme. <br>
```-v, –verbose =``` Araç içerisinde işlenen her dosya için bir tanılama çıktısı verir. <br>
```–no-preserve-root =``` ‘/’ dizinini özel olarak dizme (varsayılan kullanım) komutunu uygular. <br>
```–preserve-root =``` ‘/’ dizin üzerinde yinelemeli olarak çalışılamadı. <br>
```–reference=RFILE =``` Mod değerleri yerine RFİLE modunu kullanmamızı sağlar. <br>
```-R, –recursive = ```Dosyaları ve dizinleri yinelemeli olarak değiştirir. <br>
```–version = ```Sürüm bilgisi hakkında çıktı verir. <br>
### Sayısal İzin Tanımlama Değerleri
Linux dosya ve klasör izinleri bazı sayısal değerler ile tanımlanabilir.

Alt kısımda bulunan tabloda ```binary``` ve ```octal``` tipindeki sayısal değerlerin r (okuma), w (yazma) ve x (çalıştırma) izinleri içerisinde hangi izinleri ifade ettiğini görebilirsiniz.

Burdaki her ```binary``` sayısal değeri karşılarındaki ```octal``` sayı tipine denk gelmektedir.

En sağ kısımdaki sütün içerisinde yer alan semboller linux sistemlerindeki dosya ve klasör izinlerini temsil eden harflerdir. Eğer linux sistemlerinde dosya ve klasör izinleri konulu yazımızı okumadıysanız buradan inceleyebilirsiniz.

### Chmod Komutu İle Kullanılan Operatörler
```–```  Var olan izni kaldır (remove permission). <br>
```+```  Bir izin ekle (add permission). <br>
```=```  İzin koy (set permission). <br>
### Kullanıcılar
```u (user) =``` Dosya sahibi, dosya yaratıcısı. <br>
```g (group) =``` Dosyanın ait olduğu grup. <br>
```o (other) =``` Ötekiler, u ve g dışında olup, dosyaya erişebilenler. <br>
```a [all (u+g+o)] =``` Tüm kategoriler. <br>

### Örnekler:

Linux sistemimiz içerisinde ```izinler.txt``` isminde bir dosyamızın var olduğunu düşünelim.

Bu dosyamızın ```user (kullanıcı)```, ```group(grup)``` ve ```other (diğer kullanıcılar)``` kullanıcılarına yönelik bir izin ataması gerçekleştirelim.

izinler.txt dosyamıza yönelik izin atama işlemi gerçekleştirirken ```kali``` kullanıcısında iseniz komutumuzun başına “sudo” ifadesini eklemeniz gerektiğini unutmayın.

Dosyamıza yönelik kullanıcı, grup ve diğer kullanıcı izinlerini atama işlemi gerçekleştirirken kullanacağımız komutumuz ```sudo chmod 751 izinler.txt``` komutudur.

Kullanmış olduğumuz ```751``` ifadesi ile;

```7=``` Dosya sahibine yani u harfine rwx/read (okuma), write (yazma) ve execute (çalıştırma) yetkisi vermiş oluyoruz.
```5=``` Grup kullanıcılarına yani g harfine r-x/okuma ve çalıştırma yetkisi vermiş oluyoruz.

```1=``` Diğer kullanıcılara yani o harfine –x/sadece çalıştırma yetkisi vermiş oluyoruz.

### Bu işlemi kullanıcı sembolleri ile gerçekleştirmek istersek:

```sudo chmod u + rwx izinler.txt```

```sudo chmod g +r-x izinler.txt```

```sudo chmod o + –x izinler.txt```

Bu komutlar ile her kullanıcı için ```+``` karakteri ile izinlerin sembollerini kullanarak yetki ataması yapabiliriz.

Aynı şekilde eğer var olan yetkileri çıkartmak istersek:

```sudo chmod u – rwx izinler.txt```

```sudo chmod g – r-x izinler.txt```

```sudo chmod o – –x izinler.txt```

Sadece ```+``` karakterini ```-``` yapmamız yeterlidir.

Bu kullanıcıları ayrı ayrı belirtmek yerine tek bir komut üzerinde de belirtebiliriz.

```sudo chmod ugo – rwx izinler.txt```

buradaki kullanıcı parametreleri ve izin sembollerini değiştirmek istediğiniz kısma göre ayarlayabilirsiniz.

### Diğer İzinler
Yukarıda açıkladığımız rwx erişim hakları yerine  l, s, t harfleri gelebilir. uid (user ID) ya da gid (group ID)‘nin konulmuş olması halinde  x yerine s gelir. Bir dosya açıldığında, başka kullanıcıların onu açamaması için l izni verilir. Bu izin r, w ya da x yerine getirilebilir. Bu tür izinler, programcılar için gereklidir. Normal kullanıcılar buna gerek duymazlar. Bunların nasıl belirlendiğini görmek için, Unix Kılavuzuna bakabilirsiniz.

```l, (set locking privilege) =``` Kilitleme hakkını belirtme.
```s, (set user or group ID mode) =``` Kullanıcı ya da grup kimliği kipini belirleme.
```t, (set save text (sticki bit) mode) =``` Kaydedilen text (metin) kipini belirleme.
```u, (user’s current permissions) =``` Kullanıcının iznini belirleme.
```g, (group’s current permissions) =``` Grubun iznini belirleme.
```o, (other’s current permissions)=``` Diğer kullanıcılarının iznimi belirleme.
