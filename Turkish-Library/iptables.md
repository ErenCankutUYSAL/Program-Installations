#### Tanıtım
Iptables, Linux dağıtımları için bir yazılım güvenlik duvarıdır. Bu hile sayfası stili kılavuz, yaygın, günlük senaryolarda yararlı olan güvenlik duvarı kuralları oluşturacak iptables komutlarına hızlı bir başvuru sağlar. Bu, bağlantı noktası, ağ arabirimi ve kaynak IP adresine göre çeşitli hizmetlere izin verilmesine ve engellenmesine ilişkin iptables örneklerini içerir.

##### Bu Kılavuz Nasıl Kullanılır
Burada açıklanan kuralların çoğu, iptables'ınızın varsayılan giriş ilkesi aracılığıyla DROP gelen trafiği DROP olarak ayarlandığını ve gelen trafiğe seçici olarak izin vermek istediğinizi varsayar.
Elde etmeye çalıştığınız şey için geçerli olan sonraki bölümleri kullanın. Çoğu bölüm diğer bölümlere dayanmaz, bu nedenle aşağıdaki örnekleri bağımsız olarak kullanabilirsiniz.
İhtiyacınız olan bölümleri bulmak için bu sayfanın sağ tarafındaki İçindekiler menüsünü (geniş sayfa genişliklerinde) veya tarayıcınızın bul işlevini kullanın.
Vurgulanan değerleri kendi değerlerinizle değiştirerek verilen komut satırı örneklerini kopyalayıp yapıştırın
Kurallarınızın sırasının önemli olduğunu unutmayın. Bu iptables komutlarının tümü, yeni kuralı bir zincirin sonuna eklemek için -A seçeneğini kullanır. Eğer zincirde başka bir yere koymak isterseniz, yeni kuralın konumunu belirtmenize (veya kural numarası belirtmeden zincirin başına yerleştirmenize) olanak sağlayan -I seçeneğini kullanabilirsiniz.

Not: Güvenlik duvarlarıyla çalışırken, SSH trafiğini engelleyerek (varsayılan olarak 22 numaralı bağlantı noktası) kendinizi kendi sunucunuza kilitlememeye dikkat edin. Güvenlik duvarı ayarlarınız nedeniyle erişimi kaybederseniz, erişiminizi düzeltmek için web tabanlı bir konsol aracılığıyla ona bağlanmanız gerekebilir. DigitalOcean kullanıyorsanız, daha fazla bilgi için Kurtarma Konsolu ürün belgelerimizi okuyabilirsiniz. Konsol aracılığıyla bağlandığınızda, güvenlik duvarı kurallarınızı SSH erişimine izin verecek (veya tüm trafiğe izin verecek) şekilde değiştirebilirsiniz. Kaydedilmiş güvenlik duvarı kurallarınız SSH erişimine izin veriyorsa, başka bir yöntem de sunucunuzu yeniden başlatmaktır.

Mevcut iptables kural setinizi sudo ``iptables -S`` ve ``sudo iptables -L`` ile kontrol edebileceğinizi unutmayın.

Şimdi iptables komutlarına bir göz atalım!

##### Kaydetme Kuralları
Iptables kuralları geçicidir; bu, yeniden başlatma sonrasında kalıcı olmaları için manuel olarak kaydedilmeleri gerektiği anlamına gelir.

Ubuntu'da iptables kurallarını kaydetmenin bir yolu iptables-persistent paketini kullanmaktır. Aşağıdaki gibi apt ile kurun:
```
sudo apt install iptables-persistent
```
Kurulum sırasında mevcut güvenlik duvarı kurallarınızı kaydetmek isteyip istemediğiniz sorulacaktır.

Güvenlik duvarı kurallarınızı güncellerseniz ve değişiklikleri kaydetmek istiyorsanız şu komutu çalıştırın:
```
sudo netfilter-persistent save
```
Diğer Linux dağıtımlarının iptables değişikliklerinizi kalıcı hale getirmenin alternatif yolları olabilir. Daha fazla bilgi için lütfen ilgili belgelere bakın.

##### Listeleme ve Silme Kuralları
iptables kurallarını nasıl listeleyeceğinizi ve sileceğinizi öğrenmek istiyorsanız, şu eğiticiye göz atın: iptables güvenlik duvarı kuralları nasıl listelenir ve silinir.

##### Genel Olarak Yararlı Kurallar
Bu bölüm, çoğu sunucuda genellikle yararlı olan kurallar oluşturacak çeşitli iptables komutlarını içerir.

##### Geri Döngü Bağlantılarına İzin Verme
Lo olarak da adlandırılan geri döngü arabirimi, bir bilgisayarın ağ bağlantılarını kendisine iletmek için kullandığı şeydir. Örneğin, ping localhost veya ping ```127.0.0.1```'i çalıştırırsanız, sunucunuz geri döngüyü kullanarak kendisine ping atacaktır. Geri döngü arabirimi, uygulama sunucunuzu bir yerel ana bilgisayar adresiyle bir veritabanı sunucusuna bağlanacak şekilde yapılandırırsanız da kullanılır. Bu nedenle, güvenlik duvarınızın bu bağlantılara izin verdiğinden emin olmak isteyeceksiniz.

Geri döngü arabiriminizdeki tüm trafiği kabul etmek için şu komutları çalıştırın:
```
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A OUTPUT -o lo -j ACCEPT
```
##### Kurulan ve İlgili Gelen Bağlantılara İzin Verme
Ağ trafiğinin düzgün çalışması için genellikle iki yönlü (gelen ve giden) olması gerektiğinden, yerleşik ve ilgili gelen trafiğe izin veren bir güvenlik duvarı kuralı oluşturmak tipiktir, böylece sunucu, sunucu tarafından başlatılan giden bağlantılar için dönüş trafiğine izin verir. kendisi. Bu komut şunları sağlayacaktır:
```
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```
##### Kurulan Giden Bağlantılara İzin Verme
Tipik olarak meşru gelen bağlantılara yanıt olan tüm kurulan bağlantıların giden trafiğine izin vermek isteyebilirsiniz. Bu komut şunları sağlayacaktır:
```
sudo iptables -A OUTPUT -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
##### Dahili Ağın Harici Ağa Erişimine İzin Verme
eth0'ın harici ağınız ve eth1'in dahili ağınız olduğunu varsayarsak, bu, dahili ağınızın harici ağlara erişmesine izin verecektir:
```
sudo iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT
```
##### Geçersiz Paketleri Bırakma
Bazı ağ trafiği paketleri geçersiz olarak işaretlenir. Bazen bu tür paketleri günlüğe kaydetmek faydalı olabilir, ancak çoğu zaman onları bırakmak iyidir. Bunu şu komutla yapın:
```
sudo iptables -A INPUT -m conntrack --ctstate INVALID -j DROP
```
##### Bir IP Adresini Engelleme
Belirli bir IP adresinden, örneğin ````203.0.113.51``` gelen ağ bağlantılarını engellemek için şu komutu çalıştırın:
```
sudo iptables -A INPUT -s 203.0.113.51 -j DROP
```
Bu örnekte, ``` -s 203.0.113.51```, ````` 203.0.113.51``` kaynak IP adresini belirtir. Kaynak IP adresi, izin verme kuralı da dahil olmak üzere herhangi bir güvenlik duvarı kuralında belirtilebilir.

Bunun yerine, bağlantı isteğine ```connection refused``` hatasıyla yanıt verecek olan bağlantıyı reddetmek istiyorsanız, ```DROP``` ifadesini aşağıdaki gibi ``REJECT`` ile değiştirin:
```
sudo iptables -A INPUT -s 203.0.113.51 -j REJECT
```
##### Ağ Arayüzüne Bağlantıları Engelleme
Belirli bir IP adresinden gelen bağlantıları engellemek için, ör. ````203.0.113.51```, belirli bir ağ arayüzüne, ör. eth0, şu komutu kullanın:
```
iptables -A INPUT -i eth0 -s 203.0.113.51 -j DROP
```
Bu, -i eth0 eklenmesiyle önceki örnekle aynıdır. Ağ arabirimi, herhangi bir güvenlik duvarı kuralında belirtilebilir ve kuralı belirli bir ağla sınırlamanın harika bir yoludur.

##### Hizmet: SSH
Yerel konsolu olmayan bir sunucu kullanıyorsanız, sunucunuza bağlanabilmeniz ve sunucunuzu yönetebilmeniz için muhtemelen gelen SSH bağlantılarına (bağlantı noktası 22) izin vermek isteyeceksiniz. Bu bölüm, güvenlik duvarınızı SSH ile ilgili çeşitli kurallarla nasıl yapılandıracağınızı kapsar.

Tüm Gelen SSH'ye İzin Verme
Tüm gelen SSH bağlantılarına izin vermek için şu komutları çalıştırın:
```
sudo iptables -A INPUT -p tcp --dport 22 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 22 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
Kurulan SSH bağlantılarının giden trafiğine izin veren ikinci komut, yalnızca ÇIKIŞ politikası KABUL olarak ayarlanmamışsa gereklidir.

##### Belirli IP adresinden veya alt ağdan Gelen SSH'ye İzin Verme
Belirli bir IP adresinden veya alt ağdan gelen SSH bağlantılarına izin vermek için kaynağı belirtin. Örneğin, `````` 203.0.113.0/24`` alt ağının tamamına izin vermek istiyorsanız, şu komutları çalıştırın:
```
sudo iptables -A INPUT -p tcp -s 203.0.113.0/24 --dport 22 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 22 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
Kurulan SSH bağlantılarının giden trafiğine izin veren ikinci komut, yalnızca ÇIKIŞ politikası KABUL olarak ayarlanmamışsa gereklidir.

##### Giden SSH'ye İzin Verme
Güvenlik duvarı ÇIKIŞ politikanız KABUL olarak ayarlanmadıysa ve giden SSH bağlantılarına (sunucunuz başka bir sunucuya SSH bağlantısı başlatıyor) izin vermek istiyorsanız şu komutları çalıştırabilirsiniz:
```
sudo iptables -A OUTPUT -p tcp --dport 22 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A INPUT -p tcp --sport 22 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
##### Belirli IP Adresinden veya Alt Ağdan Gelen Rsync'e İzin Verme
```873``` bağlantı noktasında çalışan Rsync, dosyaları bir bilgisayardan diğerine aktarmak için kullanılabilir.

Belirli bir IP adresinden veya alt ağdan gelen rsync bağlantılarına izin vermek için kaynak IP adresini ve hedef bağlantı noktasını belirtin. Örneğin, tüm ````` 203.0.113.0/24``` alt ağının sunucunuzla rsync yapabilmesine izin vermek istiyorsanız, şu komutları çalıştırın:
```
sudo iptables -A INPUT -p tcp -s 203.0.113.0/24 --dport 873 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 873 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
Kurulan rsync bağlantılarının giden trafiğine izin veren ikinci komut, yalnızca ÇIKIŞ ilkesi KABUL olarak ayarlanmamışsa gereklidir.

##### Hizmet: Web Sunucusu
Apache ve Nginx gibi web sunucuları, genellikle HTTP ve HTTPS bağlantıları için sırasıyla "80" ve "443" bağlantı noktalarındaki istekleri dinler. Gelen trafik için varsayılan politikanız bırak veya reddet olarak ayarlandıysa, sunucunuzun bu isteklere yanıt vermesine izin verecek kurallar oluşturmak isteyeceksiniz.

##### Gelen Tüm HTTP'lere İzin Verme
Tüm gelen HTTP ```(port 80)``` bağlantılarına izin vermek için şu komutları çalıştırın:
```
sudo iptables -A INPUT -p tcp --dport 80 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 80 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
Kurulan HTTP bağlantılarının giden trafiğine izin veren ikinci komut, yalnızca ÇIKIŞ ilkesi KABUL olarak ayarlanmamışsa gereklidir.

##### Gelen Tüm HTTPS'lere İzin Verme
Tüm gelen HTTPS (bağlantı noktası 443) bağlantılarına izin vermek için şu komutları çalıştırın:
```
sudo iptables -A INPUT -p tcp --dport 443 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 443 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
Kurulan HTTP bağlantılarının giden trafiğine izin veren ikinci komut, yalnızca ÇIKIŞ ilkesi KABUL olarak ayarlanmamışsa gereklidir.

##### Gelen Tüm HTTP ve HTTPS'lere İzin Verme
Hem HTTP hem de HTTPS trafiğine izin vermek istiyorsanız, her iki bağlantı noktasına da izin veren bir kural oluşturmak için çoklu bağlantı modülünü kullanabilirsiniz. Tüm gelen HTTP ve HTTPS ```(port 443)``` bağlantılarına izin vermek için şu komutları çalıştırın:
```
sudo iptables -A INPUT -p tcp -m multiport --dports 80,443 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp -m multiport --dports 80,443 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
Kurulan HTTP ve HTTPS bağlantılarının giden trafiğine izin veren ikinci komut, yalnızca ÇIKIŞ politikası KABUL olarak ayarlanmamışsa gereklidir.

##### Hizmet: MySQL
MySQL, 3306 numaralı bağlantı noktasındaki istemci bağlantılarını dinler. MySQL veritabanı sunucunuz uzak bir sunucudaki bir istemci tarafından kullanılıyorsa, bu trafiğe izin verdiğinizden emin olmanız gerekir.

MySQL'e Belirli IP Adresinden veya Alt Ağdan İzin Verme
Belirli bir IP adresinden veya alt ağdan gelen MySQL bağlantılarına izin vermek için kaynağı belirtin. Örneğin, ``203.0.113.0/24`` alt ağının tamamına izin vermek istiyorsanız, şu komutları çalıştırın:
```
sudo iptables -A INPUT -p tcp -s 203.0.113.0/24 --dport 3306 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 3306 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
Kurulan MySQL bağlantılarının giden trafiğine izin veren ikinci komut, yalnızca ÇIKIŞ politikası KABUL olarak ayarlanmamışsa gereklidir.


##### MySQL'in Belirli Ağ Arayüzüne İzin Verilmesi
Belirli bir ağ arabirimine MySQL bağlantılarına izin vermek için (örneğin, özel bir ağ arabiriminiz eth1'e sahip olduğunuzu varsayalım) şu komutları kullanın:
```
sudo iptables -A INPUT -i eth1 -p tcp --dport 3306 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -o eth1 -p tcp --sport 3306 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
Kurulan MySQL bağlantılarının giden trafiğine izin veren ikinci komut, yalnızca ÇIKIŞ politikası KABUL olarak ayarlanmamışsa gereklidir.


##### Hizmet: PostgreSQL
PostgreSQL, ```5432``` bağlantı noktasındaki istemci bağlantılarını dinler. PostgreSQL veritabanı sunucunuz uzak bir sunucudaki bir istemci tarafından kullanılıyorsa, bu trafiğe izin verdiğinizden emin olmanız gerekir.

Belirli IP Adresinden veya Alt Ağdan PostgreSQL
Belirli bir IP adresinden veya alt ağdan gelen PostgreSQL bağlantılarına izin vermek için kaynağı belirtin. Örneğin, 203.0.113.0/24 alt ağının tamamına izin vermek istiyorsanız şu komutları çalıştırın:
```
sudo iptables -A INPUT -p tcp -s 203.0.113.0/24 --dport 5432 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 5432 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
Kurulan PostgreSQL bağlantılarının giden trafiğine izin veren ikinci komut, yalnızca ÇIKIŞ politikası KABUL olarak ayarlanmamışsa gereklidir.


##### PostgreSQL'in Belirli Ağ Arayüzüne İzin Verilmesi
Belirli bir ağ arabirimine PostgreSQL bağlantılarına izin vermek için (örneğin, özel bir ağ arabirimi eth1'iniz olduğunu varsayalım) şu komutları kullanın:
```
sudo iptables -A INPUT -i eth1 -p tcp --dport 5432 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -o eth1 -p tcp --sport 5432 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
Kurulan PostgreSQL bağlantılarının giden trafiğine izin veren ikinci komut, yalnızca ÇIKIŞ politikası KABUL olarak ayarlanmamışsa gereklidir.


##### Hizmet: Posta
Sendmail ve Postfix gibi posta sunucuları, posta teslimi için kullanılan protokollere bağlı olarak çeşitli bağlantı noktalarını dinler. Bir posta sunucusu çalıştırıyorsanız, hangi protokolleri kullandığınızı belirleyin ve uygun trafik türlerine izin verin. Ayrıca, giden SMTP postalarını engellemek için bir kuralın nasıl oluşturulacağını da göstereceğiz.

Giden SMTP Postasını Engelleme
Sunucunuz giden posta göndermiyorsa, bu tür trafiği engellemek isteyebilirsiniz. 25 numaralı bağlantı noktasını kullanan giden SMTP postasını engellemek için şu komutu çalıştırın:
```
sudo iptables -A OUTPUT -p tcp --dport 25 -j REJECT
```
Bu, iptables'ı ```25```` bağlantı noktasındaki tüm giden trafiği reddedecek şekilde yapılandırır. Farklı bir hizmeti 25 numaralı bağlantı noktası yerine bağlantı noktası numarasına göre reddetmeniz gerekiyorsa, yukarıdaki "25" yerine bu bağlantı noktası numarasını değiştirin.


##### Tüm Gelen SMTP'lere İzin Verme
Sunucunuzun ```25``` bağlantı noktasındaki SMTP bağlantılarına yanıt vermesine izin vermek için şu komutları çalıştırın:
```
sudo iptables -A INPUT -p tcp --dport 25 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 25 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
Kurulan SMTP bağlantılarının giden trafiğine izin veren ikinci komut, yalnızca ÇIKIŞ politikası KABUL olarak ayarlanmamışsa gereklidir.


##### Tüm Gelen IMAP'lere İzin Verme
Sunucunuzun ```143``` bağlantı noktası olan IMAP bağlantılarına yanıt vermesine izin vermek için şu komutları çalıştırın:
```
sudo iptables -A INPUT -p tcp --dport 143 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 143 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
Kurulan IMAP bağlantılarının giden trafiğine izin veren ikinci komut, yalnızca ÇIKIŞ politikası KABUL olarak ayarlanmamışsa gereklidir.


##### Tüm Gelen IMAPS'lere İzin Verme
Sunucunuzun ```993``` bağlantı noktası olan IMAPS bağlantılarına yanıt vermesine izin vermek için şu komutları çalıştırın:
```
sudo iptables -A INPUT -p tcp --dport 993 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 993 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
Kurulan IMAPS bağlantılarının giden trafiğine izin veren ikinci komut, yalnızca ÇIKIŞ politikası KABUL olarak ayarlanmamışsa gereklidir.


##### Tüm Gelen POP3'lere İzin Verme
Sunucunuzun ``110`` numaralı bağlantı noktası olan POP3 bağlantılarına yanıt vermesine izin vermek için şu komutları çalıştırın:
```
sudo iptables -A INPUT -p tcp --dport 110 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 110 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
Kurulan ```POP3``` bağlantılarının giden trafiğine izin veren ikinci komut, yalnızca ÇIKIŞ politikası KABUL olarak ayarlanmamışsa gereklidir.


##### Tüm Gelen POP3'lere İzin Verme
Sunucunuzun ```POP3S``` bağlantılarına, 995 numaralı bağlantı noktasına yanıt vermesine izin vermek için şu komutları çalıştırın:
```
sudo iptables -A INPUT -p tcp --dport 995 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 995 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
Kurulan POP3S bağlantılarının giden trafiğine izin veren ikinci komut, yalnızca ÇIKIŞ politikası KABUL olarak ayarlanmamışsa gereklidir.