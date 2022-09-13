[:house:](Home)

**Sayfa içindeki başlıklar**

[Linux Basit Kodları](http://git.medyatakip.com/system-admin/known-issues/wikis/8-Linux/Linux%20Komutlar%C4%B1#linux-basit-kodlar)<br>
[Orantılı İzin Kodları](http://git.medyatakip.com/system-admin/known-issues/wikis/8-Linux/Linux%20Komutlar%C4%B1#orantili-izin-kodlari-orantili-kod-izin-durumu-)<br>
[Karşılaştırma Kodları](http://git.medyatakip.com/system-admin/known-issues/wikis/8-Linux/Linux%20Komutlar%C4%B1#kar%C5%9F%C4%B1la%C5%9Ft%C4%B1rma)<br>
[Remove Kodları](http://git.medyatakip.com/system-admin/known-issues/wikis/8-Linux/Linux%20Komutlar%C4%B1#remove-komutlar%C4%B1)<br>
[Kalıba Göre Arama Yapma Kodları](http://git.medyatakip.com/system-admin/known-issues/wikis/8-Linux/Linux%20Komutlar%C4%B1#kal%C4%B1ba-g%C3%B6re-arama-yapma)<br>
[Paste Kodları](http://git.medyatakip.com/system-admin/known-issues/wikis/8-Linux/Linux%20Komutlar%C4%B1#paste-komutlar%C4%B1)<br>
[Komut Kodları](http://git.medyatakip.com/system-admin/known-issues/wikis/8-Linux/Linux%20Komutlar%C4%B1#komutlar)<br>
[Dokuman Hazırlama Kodları](http://git.medyatakip.com/system-admin/known-issues/wikis/8-Linux/Linux%20Komutlar%C4%B1#dokuman-hazirlama)<br>
[Yazdırma Işlemleri Kodları](http://git.medyatakip.com/system-admin/known-issues/wikis/8-Linux/Linux%20Komutlar%C4%B1#yazdirma-islemi)<br>
[Ozel Kabuk Kodları](http://git.medyatakip.com/system-admin/known-issues/wikis/8-Linux/Linux%20Komutlar%C4%B1#ozel-kabuk-komutlari)<br>

#### Linux Basit Kodlar

* `Exit:`  çıkış

* `passwd:`  En az 6 karakter.en az iki karakter alfabetik kalanin sayisal veya en az bir karakterinin sayisal olmasi gerek. 

* `logname:`  O andaki calisani user id gosterir. 

* `ls:`  Bir dizin icinde yer alan dosyalarin listesini veririr. 

* `rm *:`  Birdizin icinde yer alan dosyalari siler. 

* `>:`  Bir satira sigmayan command alt satirdan devam ettirir. 

* `&:`  Verilen komutun sonuna yazilir ve komut geri planda kendi kendine calisir.Sonuclaninca sonuclari verir. 

* `komut> dosya:`  Eger komutun olusturdugu tum sonuclar ekran yerine bir dosya icine yazilir. 

* `komut>> dosya:`  Verilen dosya isminin sonuna sonuclari yazar. 

* `;` Bu isaret commad arkasina konarak.bir satirda birden cok commad calisir 

* `komut:`  Vi programi calistiktan sonra yazma islemine baslamak icin. Satirin sonundan devam edilecek ise yine bu kullanilir. 

* `vi + dosyaadi:` `Baslangicta dosyanin en son satirina ulasmak icin bu kulla vi +UNIX 

* `dosyaadi:`  dosyada sayfalar biciminde ilerlemek icin vi -r dosyaadi:` kaybolan dosyayi cagirir.elektirik kesildikten sonrada. 

* `vi -r:`  kurtarilacak dosyalar goruntulenir. 

* `:` w dosyaismi:`  bun dosyayi yazdktan sonra kaydeder. 

* `:` 20` sati yukari cik. bu tuslara basilarak yapilir CTR d imlec komut kodundan kurtulmak icin. 

* `:` w` uzerinde calisilan metin ayni isimle saklanir. 

* `:` w dosyaadi:`  bir baska isimle saklama. 

* `:` wg| dosyadan cikmak icin. 

* `:` q| yapilan degisiklikler goz onune alinmamk isteniyorsa. 

* `:` q hic bir degisiklik yapilmamimissa terk etmek icin kullanilir. 

* `:` e| metinde degisiklik yapilmissa degisiklikler istenmiyorsa baslangic haline getimek icin kullanilir. 

* `:` w>> dosyaadi:`  yaratilan dosya, bir baska kutuge ilave edilecekse. 

* `:` n,mw>> dosyaadi:`  tumu yerine verilen satir araligi dosyaya eklenir. 

* `:` sh :`  file icinde calisiriken UNIX komutalrini kullanmamizi saglar. 

* `UNIX com:` file icinde calisiriken UNIX komutalrini kullanmamizi saglar 

* `|:` En son UNIX komutunu tekrar calistirmak icin. Vi editorunun icinde UNIX komutu kullanildiktan sonra tekrar editore donmek icin CTR-d veya :` exit gecerlidir. 

* `:` r| komut:`  UNIX komutlarina eristikten sonra bunu sonuclarini editor icin aktarir. 

* `:` w |mail burak:`  yaratilan dosyayi burak isimli kuulaniciya iletmek. 

* `CTR-f:`  Metnin bir ekran sonrasina ulasmak icin. 

* `CTR-b:` ekrani geri getirmek icin. 

* `CTR-d:`  yarim ekran yukari cikmak icin. 

* `CTR-u:`  yarim ekran asagi kaymak icin. 

* `CTR-e:`  ekrani bir sati yukari kaydirmak. basina n koyarak artabilir. 

* `CTR-y:`  ekrani bir satir asagi kaydirmak. 

* `CTR-g:` hangi satirda oldugunu bulmak icin. 

* `CTR-h:`  korsuru bir sola kaydirir.Basina yine istenilen sayi konabilir. :` Korusur bir saga kaydirir. :` Korsuru asagi dogru kaydirir. 

* `CTR-N:`  Korsuru asagi dogru hareketi saglar. :` Korusuru yukari dogru hareketi saglar. 

* `CTR-P:` Korusuru yukari dogru hareketi saglar. :` Ekran uzerindeki metnin birinci satirina hareket ettirme amaciyla kullanilir.Bu komut oncesinde sayisal deger kullanilirsa,imlec belirtile satira kayar. :` Imleci ekranin tam ortasindaki satir uzerine hareket ettirir. :` Son satir uzerine getirir. :` Imleci satir uzerindeki bir sonraki kelimenin ilk karakteri uzerin kaydirir. :` Satir uzerindeki bir onceki kelimenin ilk karakterine getirir. :` Kelimenin son karakterine ulastirir. 

* `/aranacak kelime:`  belirtilen kelimeyi ileri dogru aramayi saglar. 

* `?aranacak kelime:`  belirtilen kelimeyi geri dogru aramayi saglar. 

* `/aranilacak kelime/+n:` ` bir kelimeyi belirli bir sati araliginda aratmak. imlecin bulundugu satir ile sonraki n satiri arasinda bir kelimeyi arama /A*a: A ile baslayan ve a ile biten dizgileri bulma. :dosyanin belirli satirina ulasmak icin.bu komuttan once satir numarasi belirtilerek imlec o satira hareket eder.yoksa en sona gider. Bir once ki konuma donmek icin dur. :`  Herhangi iki karakter arasina giris yapilacak ise.ESC basinca durur. :` Imlecin bulundugu satirin hemen altina bir bos satir acmak icin. :` Imlecin bulundugu satirin uzerine satir acmak icin kullanilir. :` Bir satir uzerinde yapilan girisleri silmek icin. :` Silme islemi yanlislikla yapilmis ise kullanicinin zarara ugramamasi icin en son islemi iptal etmek uzere kullanilir. :` Satir uzerinde bir den fazla degisiklik yapilmissa ve degisiklik lerin tumunu bir den iptal etmek icin kullanilir. :` Bir satirin yok edilmesi. :` Komutun uzerinde bulunan satir dahil sonraki tum satirlar silinir :` Ekranda goruntulenen dosyanin birinci satirini siler. :` Mevcut bir karakteri 
degistirmek. : Bir karakter yerine bir den fazla karakter degistirmek icin. Yazim modundan cikmak icin ESC le cikilir. : bir kucuk harfi buyuk harfe cevirmek icin. : bir satiri kopyelemek uzere yakalamak amaci ile kullanilir.Bunun arkasindan imlece kopyalama isleminin yapilacgi konuma getirilir komutu kullanilir.:` ilgili satirin imelecin bulundugu yerin ustune kopyeler. :` belirli bir satirin kopyelenmesi bu komutla baslar. ile sonuclanir. Eger n satir kopyalanacksa ile satirlarin kopyesi bir yazmac uzerinde olusturlur ve ile istenilen yere kopyelenir. Yer degistirme islemi:` :` gibi komutlarla silme islemleri yapildiginda,silinen ifadeler yazmac uzerine kaydedilmis olur.Bu islemde sonra imlec yeni konuma getirilerek yapilir. :` En son kullanilan komutu yeniden ekrana getirir. r dosyaadi:` su anda calimakta oldugun dosyanin belirli bir bolumunu r den sonra ki dosya ismine kopyeler. chown yeni sahibinin ismi dosya:` dosyanin veya dizinin basakasina verilme i. Kisaca dosya sahibi degistiriliyor. chown yeni sahibinin ismi dizi:` 
dizi sahibi degistirme.

* `chgrp yeni grup adi dosya:` `dosya grubunu degistirme. chgrp yeni grup adi dizi:`  dizi grubunu degistirme. chmod izin modu dosya:` Bir dosyaya verilen izinlerin degistirilmesi. chmod izin modu dizi : Bir diziye verilen izinlerin degistirilmesi. 

#### ORANTILI IZIN KODLARI - Orantili kod - Izin durumu - 

`0400` - Dosya sahibi icin okuma - 

`0200` - Dosya sahibi icin yazma - 

`0100` - Dosya sahibi icin calistirma - 

`0040` - Gruptakiler icin okuma - 

`0020` - Gruptakiler icin yazma - 

`0010` - Gruptakiler icin calistirma - 

`0004` - Digerleri icin okuma - 

`0002` - Digerleri icin yazma - 

`00001` - Digerleri icin calistirma - 


* `g:`  Grup 

* `a:`  Herkes 

* `+:`  Izin vermek 

* `-:`  Izinleri kaldirmak. 

* `=:`  Belirli bir izin atamak uzere. 

* `cd dizin adi:` `dizi degistirme.. 

* `cd /:`  diziden root a gecemek icin. 

* `cd ..:`  bir ust diziye gecmek icin. 

* `pwd:` hangi dizi uzerinde calistigini goruntuler. 

* `ls:`  dizi icindeki dosyalari ve alt dizileri listeler ls (secenekler) (dosya veya dizin...) 
#### Secenekler

* `-c:`  Goruntu cok kolonlu ve dosya isimleri azalan sirada olacaktir. 

* `-` dosya isimleri sonunda `*` dizin isimleri sonunda `/` isaretleri gorun tulenerek birbirinden ayirt edilmelerini saglar. 

* `-R:`  Belirlenen bir dizin icindeki dosylar yanisira varsa tum alt dizinler icerikleriyle birlikte listeler. 

* `-a:`  .ile baslayan dosyalar dahil dizinin tum icerigini listeler. 

* `-` dosyalari siralamak veya bastirmak amaciyla `i-` dugumlerinin en son duzletme tarihlerini kullanir. 

* `-`dosyalar hakkinda daha ayrintili bilgi verir. 

* `-g:`  Eger ayrintili liste aliniyorsa yani tum bilgiler listelenecek ise ve bu listede doyanin sahibinin grup adiyla birlikte yer almasi isteni yorsa bu secenek kullanilir. 

* `-i:`  Her dosyayi idugumleri ile birlikte goruntuler. 

* `-:`  dosya isismleri virgullerle birbirinden ayrilarak listelenir. 

* `-n:`  Ayrintili listede yer alan ID numaralarini listeler. 

* `-o:`  Ayrintili listeye grup adlarinin dahil edilmesini saglar. 

* `-` dizinlerin `/` isaretiyle simgelenmesini saglar. 

* `-` dosya isimleri icinde `?` gibi grafik olmayan karakterler varsa bunla rınn listelenmesine yardimci olurlar. 

* `-r:`  Siralamayi ters yonden yapar. 

* `-s:`  Blok cinsinden dosyalarin boyutunu verir. 

* `-` dosyalarin degisime ugrama zamanlarina gore siralanmasini saglar. 

* `-` dosyalara en son erisim zamanlarina gore siralanmasini saglar. mkdir dizin adiizin yaratmak. 

* `rmdir` dizin adiizilerin silinmesi.

* `rm -r` dizin adiizi icindeki alt dizileri siler. 

* `umask nnnn:` ` Kullanici maskesi. 

* `dircmp` secenekler dizin-1 dizin-2 

secenekler:`  

-d:` her iki dizinin sahip oldugu dosyalarin karsilikli olarak goruntulenmesini saglar. 

#### Karşılaştırma

```
Ayni isimlere sahip dosyalarin iceriklerini kars lastirir,ayni ve farkli olanlari belirler,son olarakta iceriklerini goruntuler. 
```

* `-s:` Sadece farkli icerige sahip dosya isimlerinin goruntulenmesi saglar. 

* `-wn:` cikti genisligi 72 karakter olarak kabul edilmektedir.Istenirse n yerine sayisal bir deger yazilarak bu genislik degistirilebilir. 

* `cp dosya-1 (dosya-2..) dosya veya dizin:`  Bir dosyanin kopyesini olusturma 

* `cp prog1 ../per/rog1` dosyasinin per dizisine kopyelenmesi. ahmet dizisinde oldugumuzu varsayalim.yurdakul dizisindeki butun dosyal ri ahmet dizisine kopyalamak istiyoruz.su komutu kullaniriz. 

* `cp ../yurdakul/* . cp satmas dosya/:` Bulundugumuz dizinin icindeki satmas isimli dosyayi dosya isimli bir alt dizinin icine kopyalamak. 

* `mv -f dsoya-1 (dosya-2..) dosya veya dizin :`  Bir dosyanin bir diziden baska bir dizine tasinmasi. 

* `ln -f dosya-1 (dosya-2..) yeni-dosya:`  dosylarin baglanmasi. 

* `Link ln prog1 prog4:`  Ayniz dizin icinde prog1 dosyasinin prog4 isimli bir baska kopyasini olusturmak ve ona baglamak. 

* `ln prog1 muhrog1` dosyasinin muh dizini icine baglanmasi. 

#### Remove Komutları

* `-frmal` olarak kullanicinin doysa uzerinde yazma izni yok ise,silme islemi yapilamaz.Ekranda izin modu goruntulenerek silme isleminin yapili yapilamayacagi sorulur.Eger y yazilirsa sime islemi yapilir.return tusan basilirsa bu islem yapilmaz. Silme islemi yapilmadan once bu tur sorulari sorulmasi istenmiyorsa -f secenegi kullanilir. 

* `-` dosyalarin etkilesimli olarak tek tek silinmesine olanak tanir. 

* `-r:`  Bir dizini,icindeki tum dosyalarla birlikte silmek amaciyla kullanili 

* `rm * :` Butun dosyalari siler

* `rm -r:`  dizin ismi:` diziyi silmek icin. 

* `rm -fr *:`  Hem dosyalarin hemde dizinlerin herhangi bir uyariyla karsilasm dan topluca silinmesi. 

* `cat (secenekler) dosya..:` ` Bir dosyaninin iceriginin goruntulenmesi amaci ile yada iki veya daha fazla dosyanin birlestirilmesini saglar. 

#### secenekler 

* `-u:` Komut ciktisinin bir ara bellege alinmasini onler. 

* `-s:` Mevcut olmayan dosyalarla karsilasildiginda kullanici uyarilmaz. 

* `-v:` gorunmeyen bazi karakterleri gorunur hale getirir.bu secenekle birlikte asagidaki secenekler kullanilabilir. 

* `-t:` Tablarin CTR -I sek?inde goruntulenmesini saglar. 

* `-e:` Satir sonu isareti olarak dolarisaretinin goruntulenmesini saglar. 

* `cat > dosya:` Yeni bir metin dosyasi yaratmak. CTR-d ile cikilir. Bu dosyaya bir sey eklemek istersek > bunu tek basina kullanmiyoruz. Yoksa dosya yok olur . 

* `cat >> dosya:`  Bu sekilde tekrar icine girerek ekleme yapabiliriz. 

* `cat dosya-1 dosya-2 > dosya-3 :` Iki dosya birlestirilerek yeni dosya yaratilmasi. 

* `nl (secenekler) dosya:` Satirlari numaralandirarak goruntulemek. 

* `pg (secenekler (dosya..):` dosya iceriginin sayfalar halinde goruntulenmes 

#### secenekler

* `-numara:` Her defasinda goruntulenecek satirlarin sayisini gosterir. 

* `-p dizgi:` Normal olarak sayfanin en alt satirinda (isareti olasarak kullanicinin return a basmasi beklenir. 

* `-c:` Her bir sayfa goruntulenmeden once ekran temizlenir ve imlece baslan gic konummuna doner. 

* `-e:` Her dosyanin sonunda kullanicinin return a basmasi gerekmez. 

* `-n:` Normal olarak komutlar yeni satir karakteri ile son bulur.Otomatik olarak komut sonunun belirlenmesine olanak saglar. 

* `-s:` msg lerin goruntulenmesini saglar. +satir no:` Belirli bir satirdan itibaren dosya goruntulenmek isteniyorsa dogrudan satir numarasi yazmak sureti ile bu saglanir. 

* `+/kalip/:` Belirlenen kalibi iceren ilk satiri bulmak amaciyla bu tur bir tanim yapilabilir. 

* `find yol tanimi secenekler:` Belirli bir dosyanin hangi dizin icinde yerle tigini bulmak icin kullanilir. 

#### Kalıba Göre Arama Yapma

```
komutlar belirli kaliplarin bir veya daha fazla dosya icinde aranmasi icin kullanilirlar. 
```
* `-name isim:` aranilacak dosyanin ismi. 

* `-perm izin:`  Izinleri oktal olarak belirlenmis dosyalarin aranilmasi. 

* `-links n:`  linke sahip dosyalar. 

* `-user kullanici:` Belirli bir kullaniciya ait dosyalarin aranmasi. 

* `-group isim:` Belirli bir gruba dahil dosyalarin aranmasi. 

* `-atime n:` n gun icinde erisilen dosyalar. 

* `-mtime n:` n gun icinde islem goren dosyalar. 

* `-ctime n:`  n gun icinde degistirilen dosyalar. 

* `-print :`  bulunan dosyalarin ekranda goruntulenmesini saglar. 

* `grep (secenekler)` ifade (dosya..) egrep (secenekler) ifade (dosya..) fgrep (secenekler) dizgi (dosya..) 

#### Secenekler

* `-v:` aranilan kalibin bulunamadigi satirlari goruntuler. 

* `-c:` aranilan kalibin toplam kac satirda yer aldigini goruntuler. 

* `-i:` kucuk buyuk harf ayrimi yapmaz. 

* `-l:` aranilan kalibin bulundugu doys isimlerini goruntuler. 

* `-n:` Bulunan satilar dosya icindeki satir numaralari ile birlikte goruntul 

* `-b:` bulunan satirlarin blok numaralarini listeler. 

* `-s:` dosya bulunamadigi veya okunamadigi zaman hata msg leri verilmesi ist niyorsa bu secenek kullanilir. 

* `-e ifade:` - ile baslayan ifadelere izin verir. 

* `-f dosya:` Bir dosyanin icerdigi ifadeleri bir baska dosya icinde aramak amaciyla tercih edilebilir. 

* `file (secenekler) dosyalar..:` Herhangi bir dosyanin tipipni belirlemek gerekebilir.Dosyanin b`s olup olmadigi eger metin dosyasi ise turu veya dizin olup olmadigini belirlemek uzere bu komut kullanilir. 

* `-c:` Bicimlendirme hatalari icin magic dosyasinin kontrol edilmesine olana saglar. 

* `-f dosya:` Incelenecek dosya isimlerini iceren dosyayi tanimlar.Belirli dosyalarin tipi saptanacak ise ve dosya isimleri bir baska dosya icinde saklanmis ise bu secenek kullanilir. 

* `-m magic:` Alternatif bir dosya tanimlanir. 

* `file :` icinde bulunulan dizine ait tum dosyalarin tiplerini gosterir. 

* `pr secenekler (dosya..):` dosyalarin belirli duzende goruntulenmesi. 

#### Secenekeler

* `+sayfa no:` Numaralndirma isleminin baslayacagi sayfa numarasi bu sekilde belirlenir. 

* `-kolon:` cilisin kac kolondan olacagini belirler. 

* `-a:` Cikisin cokulu kolonlar bicimimde olmasi saglanir. 

* `-d:` satirlar arasinda bos satir birakilmasina neden olur. 

* `-m:` Komut ile belirlenen dosyalarin ayni ekran uzerinde ve ayri ayri kolo larda yer almasini saglar. 

* `-wn:` Sayfanin herbir satirinin kac karakter alagini belirlemek icin. 

* `-okonum:` Herhangi bir satirin baslama konumunu belirler. 

* `-ln:` Bir sayfadaki satirlarin sayisini degistirmek icin kullanilir. 

* `-hbaslik:` Yazdirilacak dosyanin basligini belirlemek amaciyla kullanilir. 

* `-Posya ekranda goruntulenirken her bir sayfa oncesinde kullanicinin return tusuna basmasi beklenir. 

* `-r:` hata raporlarinin goruntulenmesini saglar. 

* `-t:` Her sayfanin basinda ve sonunda 5 satirlik alan ayrilmasini engeller. 

* `-sayirac:` Kolonlarin belirlenen bir ayirac ile ayrilmasini saglar. 

* `dd (secenek=deger):`  dosyalarin farkli bicimlere donusturulmesi gerekti ginde dd komutulu kullanilir.Blok yapisinin degistirilmesinde,ASCII ve EBCDIC dosyalarin donusturulmesinde ve dosya icindeki harflerin buyuk veya kucuk harflere cevrilmesinde kullanilir. 

#### Conv Dosya Blok Kodları

* `if=dosya:` giris dosyasi 

* `of=dosya:` cikis dosyasi 

* `ibs=n:` Giris dosyasinin blok boyu 

* `obs=n:` Cikis dosyasinin blok boyu. 

* `bs=n:` hem giris hem de cikis dosyasinin blok uzunlugu. 

* `cbs=nonusum isleminde kullanilan ara bellegin boyutu. 

* `skip=n:` cikis dosyasi yaratilirken giris dosyasindan n blogun atlanmasi saglanir. 

* `seek=n:` kopyalama oncesinde cikis dosyasinin basindan itibaren n blogu arar. 

* `count=n:` sadece n giris blogunu kopyeler. 

* `conv=ascii:` EBCDIC dosyayi ASCII dosyaya donusturur. 

* `conv=ebcdic:` ASCII dosyayi EBCDIC dosyaya donusturur. 

* `conv=ibm:` ASCII dosyayi EBCDIC dosyaya donusturur. 

* `conv=lcase:` Buyuk harfleri kucuk harflere donusturur. 

* `conv=ucase:` Kucuk harfleri buyuk harflere donusturur. 

* `conv=noeror:` Hata durumunda donusum islemini durdurmaz. 

* `sort (-secenekler)(+pos1 (-pos2))(-o cikti dosyasi) (dosya..)osyalari belirli kolonlarina gore siralamak, bazen de ayirmak icin kullanilir. 

#### Secenekler

* `-c:` Belirlenen siralama kurallarina uygun olup olmadigini denetler. 

* `-m:` Sirali dosyalari birlestirmek amaciyla kullanilir. 

* `-u:` Birbirinin ayni olan tekrarli satirlari iptal ederek bir tanesinin goz onune alinmasini saglar. 

* `-O cikti:` Siralama islemi sonunda yaratilan yeni dosyanin adini tanimlama amaciyla bu secenekler kullanilir.Eger belirtilmesse cikis ekrana yapili 

* `-ybellek:` Siralama islemi icin gereken bellek miktari.-y0 ile en az belle kullanilacagi -y ile de en fazla bellek miktari kullanilacagi anlasilir. 

* `-zn:` n ile en buyuk kayit genisligi gosterilmektedir. 

* `-d:` Sadece alfabetik karakterlere gore siralama isleminin yapilmasini sag 

* `-f:` Buyuk ve kucuk harf ayriminin yapilmamasina neden olur. 

* `-M:` ay bilgisine gore siralama yapar. 

* `-n:` Bir aritmetik degere gore siralar. 

* `-r:` Siralamayi ters yonde yapar. 

* `-tayirac:` Alan ayiracinin tanimlanmasina olanak tanir. 

* `+pos1(-pos2):` Siralamaya temel olusturacak anahtar alaninin baslangic ve bitis pozisyonlarini belirler. 

* `uniq (secenekler) (dosya):` Bir dosya icinde birden fazla satir ayni iceri ge sahip olabilir.Alt alta tekrarli satirlarin goruntulenmesi veya goruntulenmemesi uniq komutu ile denetlenebilir. 

#### Secenekler 

* `-u:` Sadece dosya icinde tekrarlanmayan satirlarin goruntulenmesini saglar 

* `-d:` Sadece tekrar eden satilari elde etmek amaciyla kullanilir. 

* `-c:` Tekrarli satirlar tek bir satir halinde goruntulenebilir.Ek olarak ayni satirdan kac tane oldugunu herbir satirin sol tarafinda yer alir. 

* `-n:` Ilk n alan bosluklarla birlikte gozardi edilir. 

* `+n:` Islem esnasinda ilk n karakterlerin gozardi edilmesi saglanir. 

* `comm (secenekler) dosya-1 dosya-2 :` Sirali iki dosyanin ayni satirlarinin secilmesi icin kullanilir. 

* `-1:` Sadece birinci dosyada yer alan satirlari goz ardi eder ve ikinci dosya satirlarini goruntuler. 

* `-2:` Sadece ikinci dosyada bulunan satirlari goruntulemez ve birinci dosya satirlarini goruntuler. 

* `-3:` Her iki dosyada yer alan ayni satilarin goruntulenmemesi saglar. 

* `diff (secenekler) dosya-1 dosya-2osyalar arasindaki farkliliklari ortaya koyarak,gerekiyorsa degisiklikleri yapmak olasidir. 

#### Secenekler

* `-b:` Takip eden bosluklarin goz ardi edilmesine ve diger bosluk dizgileri nin esit bicimde karsilastirilmasina olanak saglar. 

* `-e:` a,c ve d komutlarinin kullanilmasini saglayan bir editor olusturur. Boylece iki dosya arasindaki farkliliklarin ortadan kaldirilabilmesi icin ortam hazirlar. 

* `-f:` yine bir editor olanagi saglar.Ama e secenegi kadar kullanisli degil. 

* `-h:` Iki dosya arasindaki farkliliklari hizli bicimde goruntuler. 

* `bdiff dosya-1 dosya-2 (secenekler):` Karsilastirilacak dosya cok buyuk ise bu komuttan yararlanilir.Ayni olan satilari goruntulemez farkli satirlari kucuk parcalara ayirarak herbiri uzerinde diff komutunu uygula Aksi belirtilmedikce dosya 3500 satirlik paracalara ayrilir. 

#### Secenekler 

* `-` dosyanin kac satirlik parcalara ayrilacagini belirler. 

* `-` degisikligin kabul edilmemesini saglar. 

* `diff3 (secenekler) dosya-1 dosya-2 dosya-3:` Uc dosya arasindaki farklilik lari ortaya koymak ve duzelmetleri yapmak icin kullanilir. 

* `tail (secenekler) (dosya):` Bir dosyanin sonundan belirli sayida satiri secerek goruntulemek olasidir.Satirlar veya bloklar seklinde olabilir. 

#### Secenekler 

* `+` sayi dosyanin basindan itibaren baslayarak goruntulenecek birim sayisi. 

* `-` sayio dsyanin sonundan itibaren baslayarak goruntulenecek birim sayisi. 

* `l:` Satirlara gore secme islemi. 

* `b:` Bloklara gore secme islemi. 

* `c:` Karakter sayisina gore secme islemi. 

* `f:` Buyuyen bir dosyanin satirlarini goruntuler. 

* `split (-n) (dosya(isim)):` Bir dosyayi belirlenen sayida ayri dosyalara bo mek amaciyla bu komut kullanilir. 

* `Csplit (secenekler) dosya argumanlar..osyalari bazi argumanlara gore parcalara ayirabilir.n+1 kesime ayirir. 

* `-s:` Csplit komutu yaratilan her dosya icin karakter sayisini goruntuler. -s yazilirsa bu islemi yapmaz. 

* `-k:` Csplit kullanirken bir hata ortaya ciktiginda yaratilan dosyalar sili nir.-k bu durumu engeller. 

* `-f oneki:` xx00,xx01,...xxn bicimindeki isimler yerine istenilen oneklerin verilmesi amaciyla -f secenegi kullanilir. 

* `Arguman /ifade/:` Bu ifadelerin ana dosya icinde yer aldigi konum bulunarak bu ko numdan onceki tum satirlar xx00,kalan kisimlar ise xx01 dosyasi icine yerlestirilir. 

* `%ifade%:` Yukaridaki gibi islem gorur.Ama bu konuma kadar olan birinci bo lum icin yeni bir doya yaratilmaz. 

* `satir-no:` Ana dosyanin belirlenen satir numarasina kadar olan kesim alini 

* `cut secenekler (dosya..):` Bir dosya icindeki satirlarin icerdigi alanlari keserek belirli bir yere kopyalamak. 

#### Secenekler

* `-cliste:` Karakter konumlarini belirler. 

* `-fliste:` Ozel bir ayirac ile ayrilan alanlari tanimlamak amaciyla kullani 

* `-dayirac:` Ayircalari tanimlamak uzere kullanilir. 

* `-s:` -fseceneginin kullanildigi durumlarda ayiraca sahip olmayan satirlari atlanmasini saglar. 

#### Paste Komutları
```
paste secenekler dosya-1 dosya-2:` Ayni bir dosyanin veya baska dosyalarda belirli satirlari birlestirerek bir satir elde etmek icin kullanilir. 
```

* `-dkarakter:` Iki dosyanin karsilikli satirlarini araya belirlenen karakter yerlestirmek suretiyle birlestirilir. 

* `-sosya icindeki satirlari yan yana birlestirir. 

* `-osya ismi yerine kullanilarak satirlarin ekrandan girilmesini saglar. 

* `join (secenekler) dosya-1 dosya-2:` Iki ayri sirali dosyanin iliskili kayitlarini bir satir uzerinde birlestirmek amaciyla kullanilir. 


* `-an:` Normal cikisa ilave olarak n numarali dosya icindeki eslesmeyen sati larda goruntu uzerinde yer alir.n 1 veya 2 olabilir. 

* `-e dizgi:` Bos cikti alanlarina tanimlanan dizginin yerlestirilmesini sagl 

* `-jn m:` n numarali dosyanin m ninci alani uzerinde birlestirme yapilir. 

* `-o n.m:` n numarali dosyanin m alanini birlestirir. 

* `-tayirac:` Alan ayiracinin tanimlanmasini saglar. 

* `tr (secenekler) (dizgi-1 (dizgi-2)):` Giris bilgilerinin icerdigi karakter leri baska karakterlerle degistirmek veya silerek cikis elde etmek amaci lakullanilir. 

* `-c:` Birinci dizginin icerdigi karakterler disinda kalan tum karakterleri, ikinci dizgide belirtildigi gibi degistirir. 

* `-d:` Birinci dizgi icinde belirtilen tum giris karakterlerini siler. 

* `-s:` Tekrarli karakterleri tek karaktere donusturur. 

* `wc (secenekler)(dosya..):` Bir dosyanin kac satirdan olustugunu gosterir. Bunun yaninda Dosyanin icerdigi karakter veya kelime sayilari ogrenilir. 

#### Secenekler 

* `-l:` Satirlari saymak amaci ile bu secenek kullanilir. 

* `-w:` Kelimeleri saymak icin kullanilir. 

* `-c:` Karakterleri saymak icin kullanilir. 

* `od (secenekler) (dosya):` Bir dosyanin icerigi sekizlik duzende goruntulen mek istenebilir. Ozellikle grafik ozellikli olamyan gorunmez karakterler dosya icindeki yerlerini belirlemek acisindan kullanilir. 

* `-b:` Herbir bayti sekizlik duzende gosterir. 

* `-c:` Her bir bayti ASCII karakterlerle goruntuler.Bu arada gorunmez karak terler ozel bicimde yer alir. 

```
 /(bunun tersi)0 null /b geri bosluk /f sayfa basi /n yeni satir /r return /t tab 
```

* `-d:` Herbir kelime desimal olarak simgelenir. 

* `-o:` Her kelime sekizlik duzende goruntulenir. 

* `-x:` Herbir kelime onaltilik karakterler biciminde simgelenir. 

* `-s:` 16 bit kelimeleri isaretli desimal karakterler biciminde yorumlar. 

* `lp (secenekler) dosyalar:` Basilabilir doyalarin yaziciya gondermek.Burda dosya hemen yazdirilmayarak spool a atilacaktir. 
Secenekler:`  
* `-c:` Lp komutu kullanildiginda yazdirilacak dosyanin bir kopyasinin olus turulmasi isteniyorsa -c secenegi kullanilir.bu secenek kullanilmadigind kopyalama islemmi yerine link islemi gerceklestirilir. 

* `-dyazici:` Yazma isleminin yapilacagi yazicinin veya yazicilar sinifinin belirlenmesi amaciyla bu secenek tercih edilir. 

* `-:` dosyalarin yazicidan bastirilmasi ardindan measj gonderilmesine olanak saglar. 

* `-nsayi:` Yazdirilacak dosyanin kopya sayisini saprtamak uzere bu secenek ten yararlanilir.kullanilmassa 1 oldugu varsayilir. 

* `-s:` lp den "request id is.."gibi mesajlarin atilmasini saglar. 

* `-tbaslik:` Yazicidan alinan cikisa bir basligin yazdirilmasi isteniyorsa bu secenek kullanilir. 

* `lpstat (secenekler):` Yazicidan alinmak uzere gonderilen dokumlerin duru munu ogrenmek uzere bu komut kullanilir. 

#### Secenekler 

* `-a(liste):` Listede belirtilen yazicilara gonderilen istekler hakkinda bilgi verir. 

* `-c(liste):` Yazici siniflari ve onlarin uyelerini goruntuler. 

* `-d:` sistemin kabul ettigi yaziciyi goruntuler. 

* `-o(liste):` Yazdirilmak uzere gonderilen dokumleri goruntuler.Liste yazici isimlerini siniflari ve istekleri kapsar. 

* `-r:` Lp istek tablosunun durumunu goruntuler. 

* `-s:` Sistemin kabul ettigi yazicinin ismi ver herbir yazicinin sahip oldugu ozel dosya isimlerini goruntuler. 

* `-t:` Tum durum raporunu goruntuler. 

* `-u(liste):` Liste icinde belirtilen kullanicilara iliskin durum raporunu goruntuler. 

* `cancel (liste numarasi) (yazicilar):` Lp komutuyla yazdirilmak uzere spool a gonderilen basim istekleri,gerektiginde daha bastirilmadan iptal edilebilir. Hangi listelerin spoolda bekledigini lpstat komutu ile ogrenebiliyorduk. 

* `disable (secenekler) yazicilar:` Bir yaziciyi veya bir grup yaziciyi devre disi birakmak olasidir.Bu takdirde yazim istekleri yerine getiril meyecektir. 

#### Secenekler:

* `-c:` Su anda yazicidan basilmakta olan istekleri iptal etmek uzere kullan. 

* `-r(nedeni#):` Yazicinin neden iptal edildigini kaydetmeye yaramaktadir. 

* `enable yazicilar:` Yaziciyi aktif hale getirmek 

* `ps (secenekler):` Islemlerin durumlari hakkinda bilgi edinmek icin kullanilir.Aktif islemler hakkinda cesitli bilgiler verir. 

#### Secenekler:`  

* `-e:` Tum islemlerle ilgili bilgilerin goruntulenmesini saglar. 

* `-d:` Grup liderleri haric tum islemler hakkinda bilgi verir. 

* `-a:` Grup liderleri ve terminallerle iliskisi olanlar disinda kalan tum islemler hakkinda bilgi verir. 

* `-f:` Tam listenin uretilmesine olanak tanir. 

* `-l:` Ayrintili listeyi goruntuler. 

* `-c dosya:` /dev/swap in bulundugu yerde swapdev dosyasini kullanir. 

* `-t liste:` Liste icinde yer verilen terminallerden yurutulen islemler hakkinda ozel bilgi saglar. 

* `-p liste :` Listede ID numaralari tanimlanan islemlere iliskin ozel bil gileri goruntuler. 

* `-u liste:` Belirlenen kullanicilarla iliskili islemlere ait ozet bilgi. 

* `-g liste:` Grup liderlerinin iliskili oldugu islemler hakkinda o bilgi. 

* `kill (-sinyal) PID..:` Calismakta olan bir islemi kesmekte kullanilir. PID numaralari ps komutu ile gorunur.Bir cok sintal tanimlanabilir. Bunlardan -9 islemi oldurur. 

* `nice (-sayi) komut (argumanlar):` Kullanicilarin kendi yarattiklari islemlerin onceliklerini dusurebilirler.Kullanici islemlerin onceligini yukseltemez.Bu olanak sadece sistem yoneticisine taninmistir.Sayi 19 dan fazla olamaz.Bu olay islem baslamadn yapilmadir. 

* `date:` gunun tarihi ve saatini gosterir. 

* `cal ((ay) yil):` Belirli bir aya veya yila ait takvimi elde etmek icin kul lanilir.cal komutu direk kullanildiginda bulundugun ayi ve yili gosterir 

* `cal yil :` Istenelin yilin butun aylarini gosterir. 

* `calendar:` Kullaniciya o gun yapmayi planladigi isleri hatirlatmaktadir. Bu komut kullanicinin kendi dizininde yer alan calendar isimli dosyayi okur.Bu dosyaya hatirlatilacak islerle ilgili zaman ve aciklamalari iceren bilgiler girilmelidir. 

* `.profile` dosyasinin icine bu komut konur. 

* `bc:` Hesap makinasi.Bazi fonksiyonlar tanimlanabildigi gibi istenirse -l secenegi kullanilmak suretiyle matematik kitaplik fonkisyonlarina ulasi labilir.Bu sayede ,trigonometri,logaritma ve ozel fonsiyonlar dogrudan dogruya kullanilabilir. 

* `mail (secenekler) mail (secenekler) kullanici :`  mail gonderme.mail yazimi bittikten sonra . isareti konur ve return e basilir. 

#### Secenekler: 

* `-e:` Mesajin ekranda goruntulenmemesini saglar. 

* `-p:` Ekranin en alt satirinda ? isareti goruntulenmez.msg ler arka arkaya akip gider. 

* `-q:` Kesme isleminin yapilabilmesine olanak saglar. 

* `-r:` Mesajin ilk giren ilk cikar ilkesine gore goruntulenmesini saglar. 

* `-f dosya:` Mesajlarin belirtilen dosya icine kayit edilmesini saglar. mail kullanici kullanici kullanici 

* `.. :` Bir den fazla kisi ye ayni mail i bu sekilde gonderebiliriz. mail kullanici at node:` Uzak yere mail gondermek. 

* `mail :` gelen mailigi gorebiliriz.Ve icinden su komutlari yazabiliriz. 

#### Komutlar

* `+:` Msg lari ileri dogru aramak amaciyala kullanilir. 

* `-` geri dogru goruntulemek amaciyla kullanilir. 

* `d:` Belirtilen msg nin silinmesi 

* `p:` msg yeniden goruntulenmesini saglar. 

* `q:` Silinmemis msg lerin mailfile dosyasi icine geri yukleyerek mail komut nun kullanimina son verir. 

* `x:` Mesaji degistirmeden cikisa son verir. 

* `*:` komutlarin bir ozetini goruntuler. 

* `s(dosya):` msg nin belirlenen dosya icine kaydedilmesini saglar. 

* `w(dosya):` msg leri belirtilen doyanin icine basliklar dahil olmamak uzere saklar. 

* `m(kullanici):` msg nin belirtilen kullaniciya gonderilmesini saglar. 

* `mailx:` Msglerin elektronik olarak gonderilmesi ve alinmasi icin ortam saglayan bir yazilimdir.Mailx komutu asagidaki olanaklari saglar. 

* `Komutlar:` Msg okunurken saklama, silme ve msgleri yanitlama olanagi sagla 

* `Tilde Komutlari:` Msglerin gonderilmesi asamasinda, cogunlukla duzeltmeler olanak saglamak amaciyla tilde komut?arindan yararlanilabilir. 

* `Cevre degiskenler:` Mailx msg leri bicimlendirmeye yarayan bircok cevresel degisken sunmaktadir. Gelen msgler mailbox isimli standart bir dosya icinde saklanir.msgler okundugunda,saklanmasi amaciyla ikinci bir dosya daha kullanir.Ikinci dosya mbox dir. 

* `mailx (secenekler) (isim..):`  

#### Secenekler:  

* `-e:` Msgnin varligini kontrol etmek icin kullanilir. 

* `-fkutuk:` Msglerin okunacagi dosyanin adini belirler.Eger tanimlanmassa MBOX olarak kabul edelir. 

* `-H:` Sadece basliklarin ozet halinde goruntulenmesini saglar. 

* `-i:` Kesilmeleri goz ardi eder. 

* `-n:` mailx.rc deki tanimlarin goz onunen alinmamasini saglar. 

* `-N:` Ilk ozel basliklarin bastirilmamasini saglar. 

* `-r adres:` Adresleri network dagitim yazilimina gecirir.Tum tilde komutla rinin iptal edilmesini saglar. 

* `-u kullanici:` Belirtilen kullanicinin msg dosyasini okur. 

* `write kullanici (terminal tanimi):` Msg nin iletildiginde aninda ekranda olmasini saglar. 

* `mesg n:` Kendine gelen msglarin iletilmemesini saglar. 

* `news (secenekler) (maddeler):` Gunluk olaylardan haberdar olmak icin. 

* `mesg (n) :` Kullanicinin msg sini kapatmasi. 

* `mesg (y):`  Kullanicinin msg sini acmasi. 

#### DOKUMAN HAZIRLAMA 

```
nroff (secenekler) (dosyalar)okumanlarin istenilen bicimde hazirlanmas amaciyla kullanilir.Her seyden once dokumanlarin kaydedildigi bir dosya nin yaratilmasi gerekir.Yazdirlacak metinler ve bicimlendirme komutlari bu dosya icinde yer alacaktir. 
```

* `-oliste:` Sadece liste icinde belirtilen sayfalarin dokulmesini saglar. 

* `-dokumanin basliginda n. sayfaya kadar n- ise n. sayfadan dokumanin sonuna kadar olan tum sayfalarin bastirilmasini saglar. 

* `-nr:` Sayfa numaralarinin r den baslamasini saglar. 

*  `-sn:` Her n. sayfada dokum isleminin durdurulmasini saglar.Bu sayede kagit yerlestirme veya degistirme islemleri yapilabilecektir. 

* `-rcn:` c yazmacinin(register) n e atanmasini saglar. 

* `-dosya bosaltildiktan sonra standart giristen okuma yapar. 

* `-q:` rd istekleri icin esanli giris-cikis modunu cagirir. 

* `-Tterminal:` Belirlenen terminalden cikisin alinmasini saglar. 

* `-w:` Yazici mesgul ise beklenmesini saglayacaktir. 

* `-b:` Yazici mesgul ise rapor eder. 

* `-z:` Sadece .tm tarafindan ortaya cikarilan msg leri basar. 

* `-a:` Kelimeler arasinda esit bosluklar birakilmasini ve satirlarin ayarlan masini saglar. 

```ssh
Nroff ile birlikte kullanilan alt komutlar:  Satir atlatma: Metin arasinda istenilen yerlerde bos bir satir atlatilabilir. 
(.sp n) komutu kullanilir. Metni ortalama: Metindeki ifadeleri ister bir kelime olsun isterse cumle olsun sayfa uzerinde 
otomatik olarak ortalanabilir.(.ce n) ile. Soldan bosluk birakmak:` Metin icinde bazi satilarin solunda bosluk birak mak gerekebilir.
Ozellikle yeni pragraf bosluklari icin tercih edilebilir (.ti n) komutu yardimi ile olur.
 Satirlarin altini cizmek: Bir satirin icerdigi tum kelimelerin alti cizdi rilebilir.(.ul n) kullanilir.
 Istenirse bir kelime veya bir kelime grubu nun alti cizilebilir.Alti cizilecek kelimenin ayri bir satira kaydedilmasi 
 ve (.ul) komutunu kullanmak gerek. Satir genisliginin belirlenmesill.ni) komutu araciligiyla olmaktadir. ni satir genisligini 
 inch olarak belirtmektedir. Sayfa genisligi:` Sayfa boyunun 66 satir oldugu kabul edilir.(.pl n) komutu ile degistirilebilir.
 n ile sayfanin boyu gosterilmektedir. Satir bosluklari:` Her satir arasinda otomatik olarak bi
r b`s satir olur. Istenildigi kadar bosluk yaratmak icin (.ls n)komutu kullanilir. Satir ayarlamalari:
 Metin icindeki ifadelerin satiri dolduracak bicimde yerlesmesi isteniyorsa(.fi) komutu kullanilir.
 Bu islemin yapilmamasi isteniyorsa(.nf) komutu kullanilir. Dosya oldugu gibi dokturulmesi amaclaniyorsa
 (.fi) ve(.nf) komutlari birlikte kullanilir. 
 Sol taraftaki bosluklar: Bastirilacak metnin sol tarafinda istenildigi kadar bos yer birakilabilir.
 (.po n)komutu kullanilir. Sag tarafa yanastirmak.na) komutu kullanilir.Eski hale dondurmek icin (.ad) komutu kullanilir. 
```

#### Yazdirma islemi
```
norff dosyaismi (dik cizgi)page nroff dosyaismi(dik cizgi)lpr -ms makrolari: 
 standart pragraflar: Birinci satir digerlerine nazaran biraz daha icerden baslamaktadir.
 Bunu saglamak icin (.PP) kullanilir. Sol tarafa dayali paragraflar: .(.Lp) komutu kullanilir.
 Soldan bosluga sahip paragraflar.lp) komutu kullanilir. Sol ve sagdan bosluga sahip paragraflar.QP) komutu kullanilir. 
 Ters standart paragraflar.PP)komutu ile standart paragraflar elde edil biliyordu.bunun tam tersi (.XP) komutudur.
 Siradan basliklar: Metin icindeki bazi bolumler basit basliklar olarak tanimlanabilir.
 Bunu saglamak icin (.SH) Numarali basliklar.NH)komutu kullanilir. 
 Sayfa kontrolu:  yeni sayfalarin baslangici: Metnin herhangi bir yerinden itibaren kalan kisimlarini yeni bir sayfadan baslatabiliriz. 
 (.bp) komutu kullanilir. Sayfa genisliginin ayarlanmasi:  (.pl ni) komutu ile olur. 
 Baslik ve dipnot bosluklarinin degistirilmesi.nr HM ni) (.nr FM ni) Baslik ve dipnotlarin yazdirilmasi:
 .ds LH: Basliklari sola dayali yazar. .ds CH: Basligin ortalanmasini saglar. 
```

```
.ds RH %: Basligin saga yanastirilmasini saglar.%ile sayfa numarasinin yazma pozisyonu gosterir. 
.ds LFipnot ifadesini sola dayali yazar. .ds CFipnotu ortalar. .ds RSipnotu saga yanastirir. 
Dokumanlarin kapaklari ve dizin bolumlerinin hazirlanmasi:  Kapak sayfasinin tanitimi.RP)komtu kullanilir.
Baslik.TL)komutu kullanilir. Yazar isimleri: Eger birden fazla yazar ismi varsa (.AU) komutu kullanili Yazarlarin calistiklari kurumlar.
AI) komutu kullanilir. Dokuman ozeti.AB)komutu kullanilir.(.AE)ozetin sonunu belirler. Icindekiler tablosu: -ms komutlari tercih edilir.
.XS n: Tablonun ilk satirini tanimlar.n belirtilen konunun basladigi satiri belirler.
.XA n: Birinci satir disinda kalan diger tum konular ve sayfa numaralarin tanimlar.
.XA n m : Satirin belirli kolonlardan baslamasi isteniyorsa,sol taraftan bosluk birakilacaksa bu tanim yapilir. 
.XE: Tablonun sona erdigini belirlemek uzere kullanilir. .PX: Bu komut nroff a dokumanin bu bolumunun icindekiler tablosu oldugunu tanitir. Mektuplarin bicimlendirilmesi.
```

* `.rd:` Mektuplarin bazi bolumlerinin standart giris ortamindan tanitilmasin saglar.
* `.nx liste:` Nroff komutunun bicimlendirme islemine baslamasina neden olur. Mektuplarin gonderilecegi tum adresilerin okutulmasi ve herbiri icin met nin duzenlenmesi saglanir. 
* `.ex:` Nroff dan cikisi saglar.Listeyi iceren dosyanin sonunda yer aldigi takdirde,dongunun kesilmesini saglar.
 * `.TS:` Tablo tanimlarinin metin icinde nerden basladigini belirtir. 
 * `.TE:` Tablo tanimlarinin bitis noktasini belirten komuttur.
 * `tab(+)` bu isaret kolonlarin birbirinden ayrilmasini saglar. 

* `c:` Tablo elamaninin ortalanark yazdirilmasi amaciyla kullanilir. 

* `r:` Belirtilen tablo elamanlarinin saga dayali olarak yazdirilmasini sagla 

* `l:` Sola dayali yazimi saglar. 

* `n.:` Ondalik noktanin yerlestirilmesi amaciyla kullanilir. Kabuk Programlari:`  BOURNE kabugunda calisirken 

* `C kabuguna gecme:`  csh %_ komutunu bu sekilde kullaniriz.C kabugunun komutu kabul isareti % olarak degismistir. 

```
Sinirlandirilmis kabuk tanimi rsh komutu ile yapilabilir.
Bu durumda kull nici UNIX komutlarini kullanma acisindan oldukca sinirlandirilmis olur. 
Sinirlandirilmis kabukta dizinlerin degistirilmesi,PATH degiskeninin ye niden belirlenmesi, tam yol tanimlari
 ve >ile >> yonlendirme islemleri kisitlanmistir. Kabuk programlarinin yaratilmasi ve calistirilmasi: 
 kabuk programlarinin vi editoru ile yazabiliriz.Program yazildiktan sonra calistirmak icin iki yoldandan biri tercih edilebilir.
 Birincisi sh kabuk-programi(parametreler) biciminde.Diger yol ise bu dosyayi calis tirilabilir dosya haline donusturmektir.
 Bir dosyayi calistirilabilir hal getirmek icin chmod komutu ile izinlerini degistirmek gerekiyordu. chmod a+x kabuk-dosyasi 
 komutu ile bu olanagi saglayacaktir. Ornek:` Sistemede kullanicilarin sayisini belirlemek uzere, $who(dik cizgi)wc -l komutu kullaniliyordu.
 Komutlari bir kabuk dosyasi icine atarak bu dosyayı yeni bir komut gibi calistirmak olasidir.
 vi editorune $ vi say biciminde girilerek yukaridaki komutlar kaydedilir.
 Ciktiktan sonra $sh say ile dosya calistirilir. Bir diger yol chmod a+x say ile calisabilir 
 dosya elde edilir.Calismak icin dogrudan dogruya dosyanin adini yazmak yeterlidir. 
 $say Kisa kabuk dosyalarini ilk kez yaratmak icin vi yerine cat komutu da kullanilabilir.
 $cat >say komutu bu sekilde yazildiktan sonra return tusuna basilarak bir alt satira gecilir.
 Komutlar bu satirdan itibaren kaydedilir.
 Islem tamamlandiginda ctrl+d tuslarina basilir.
 $cat >say Who(dik cizgi)wc -l ctrl-d $ Aciklama satirlari: 
 Bu satirlar # isareti ile baslamak zorundadir.Aciklama satirlari prog ram icine asamalarini veya gerekli aciklamalari
 yerlestirmek ve belgele me amaciyla kullanilir.Aciklama satirlari program calisirken herhangi bir islem gormez. 
 Aciklamalar # isareti ile baslamak kosulu ile program in herhangi bir yerine yerlestirilebilir. 
 Asagidaki programda yer alan ilk iki satir islem gormeyecektir. $cat yoket #yoket programi 
 #Bu program bazi dosyalarin silinmesini saglar rm/usr/acct/muhasebe/gecici* rm/usr/acct/personel/gecici* $ 
```

#### Ozel kabuk komutlari

* `exec:` Kabuk programlari icinde yeni bir islem yartmaksizin komut calisti rilabilmesine olanak tanir. exec (argumanlar...) biciminde tanimlanir. 

* `newgrp:` Grup tanimini degistirmek uzere newgrp komutu kullanilir. newgrp (-) (grup) biciminde kullanilir.Eger - secenegi kullanilirsa grup baslangictaki haline donusur. 

* `set:` En basit sekliyle yani herhangi bir secenek ile birlikte kullanil madigi takdirde cevre degsikenlerini topluca goruntulemek amaciyla terci edilir. 

#### Secenekler 

* `-a:` Export icin degistirilecek yada yaratilacak degiskenleri isaretler 

* `-v:` Kabuk tarafindan okunmakta olan satirlarin goruntulenmesini saglar. 

* `-x:` Komutlarin ve onlarin calistirilan argumanlarini goruntuler. 

* `unset:` Mevcut tum cevre degiskenleri yok etmek amaciyla yararlanilir. 

* `ulimit:` Herhangi bir secenek kullanilmadiginda veya -f secenegi tercih edildiginde kabuk ve onun yavru islemi tarafindan yaratilan dosyalarin boyutlarina bir sinirlama getirmek uzere kullanilir. 

* `ulimit (-f) (n) break:` Kabuk programlarinda kullanilan for,until veya while gibi komutla rin olusturdugu dongulerden kurtulmak gerektiginde bu komut kullanilir. 

* `continue:` Break komutunun ters islemi continue ile gerceklestirilir. 

* `echo:` Belirtilen ifadeleri goruntulemek uzere echo komutunundan yararla nilir. echo (argumanlar) biciminde tanimlanir.Asagidakiler kullanilabilir. /(tersi)b geri bosluk /(tersi)c yeni satira baslamadan yazamaya devam eder. /(tersi)f yeni satir /(tersi)r return tusu /(tersi)t tab /(tersi) ters slash /n(tersi)n sifirla baslamasi gereken 1,2 veya 3 haneli ASCII kodlarin 8 bit karakteri. /(tersi)v dikey tab 

* `exit:` Kabuk programinin herhangi bir yerinde programlardan cikilmasi iste niyorsa exit komutundan yararlanilabilir. exit (n) biciminde tanimlanir. Kabuk programina Return kodunun gecirilmesi amaciylada kullanilabilir. Exit komutu bir kabuk programinda dogru calistigi zaman 0 yanlis calis tigi zaman sifirdan farkli bir sayi uretmesine neden olur. 

* `export:` Bir komut yorumlayicisindan bir baskasina gecildiginde, degisken lerin degerlerinin de bu yorumlayiciya aktarilmasi isteniyorsa export komutunudan yararlanmak gerekiyor. read:` Kabuk programlarinda yer alan degiskenlere program disindan ve klavye yardimiyla bilgi atanmasini saglamak uzere kullanilir. 

* `readonly:` Bir degisken okunduktan sonra artik yeni bir deger almaz.Sadece okunmak uzere cagrilabilir. readonly degisken.. biciminde kullanilir.Bu tur tanimlanan degiskenleri goruntulemek uzere dogrudan dogruya readonly komutu kullanilir. 

* `return:` Bir fonsiyonun belirlenen bir return koduyla cikmasina neden olur return (n) biciminde kullanilir. n arzu edilen bir return kodudur.Eger belirtilmesse en son calistirilan komutun return durumunu goruntulenir. 

* `shift:` Konumsal parametreler bilindigi gibi $0 ile $9 arasinda 10 adet idi.Bu komtu kullanarak ilk parametre gozardi edilerek numaralandirma yeniden yapilir.Boyle parametre sayisi bir artmis olur. 

* `test:` Bir ifadenin mantiksal degerini alacagi dogru veya yanlis durumla rina gore ozellikle dongu komutlarini kontrol etmek uzere kullanilir. Test komutu ile birlikte kosullari belirtmek uzere kullanilabilecek argumanlarin bazilari asagida yer almaktadir. 

* `-r dosya:` Belirlenen dosya mevcut ise ve kullanici tarafindan okunabilir durumda ise dogru. 

* `-w dosya:`dosya mevcut ise ve kullanici tarafindan yazilabilir ozellikle re sahip ise dogru 

* `-x dosya:` dosya mevcut ise ve calistirilabilir durumda ise dosgru. 

* `-s dosya:` dosya mevcut ve ici dolu ise dogru. 

* `-d dosya:` Eger dosya bir dizin ise dogru. 

* `-f dosya:` Mevcut dosya siradan bir dosya ise dogru. 

* `-p dosya:` dosya mevcut ise ve bir pipe(fifo) dosyasi sie dogru. 

* `-z dizgi:` dizginin uzunlugu sifir ise dogru. 

* `-n dizgi:` Sozkonusu dizginin uzunlugu sifirdan farkli ise dogru. d1=d2:` d1 dizgisi ile d2 dizgisi

 

 

