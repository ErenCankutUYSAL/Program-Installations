## CentOS Linux 8'den CentOS Stream 8'e Nasıl Geçilir

CentOS 7'den CentOS 8'e yükseltmeyi planlıyorsanız, CentOS 8 kullanımdan kaldırılacağı için şimdilik bunu atlamanız gerekebilir! Zaten kullanıyorsanız, CentOS Linux 8'den CentOS Stream 8'e geçmeyi düşünmelisiniz.

CentOS (Topluluk ENTerprise İşletim Sistemi'nin kısaltması), Red Hat Enterprise Linux sisteminin (RHEL) klonudur. CentOS, istikrarı ve güvenilirliği ile yaygın olarak bilinir ve birçok web barındırma sağlayıcısı için popüler bir seçimdir. Ayrıca RHEL'i ücretsiz olarak öğrenmek isteyenler için bir kapıdır. Gösteri bitti. CentOS geliştiricileri, odaklarını CentOS Akışına kaydırdıklarını duyurdular.


 
Resmi duyuruya göre, RHEL 8'in yeniden inşası olarak CentOS Linux 8, 2021'in sonunda sona erecek. CentOS Akışı, bu tarihten sonra Red Hat Enterprise Linux'un yukarı akış (geliştirme) dalı olarak hizmet vermeye devam edecek. Başka bir deyişle, CentOS Stream, yuvarlanan bir yayın öncesi (yani beta) modeli olacak.

Böylece CentOS Stream, artık RHEL sürümünün aşağı yönlü yeniden inşası olmayacak. Artık Fedora ve RHEL arasında yaşayacak bir orta akım. Bunu sıradan terimlerle ifade etmek gerekirse, artık Fedora -> RHEL -> CentOS değil, Fedora -> CentOS -> RHEL. Ocak 2022'den itibaren RHEL, CentOS'a dayalı olmayacak.

31 Aralık 2021'e kadar CentOS 8'i kullanmaya devam edebilir ve yamalar gönderebilirsiniz. Ancak CentOS 8, gelecek yılın bu zamanlarında erken sona erecek ve CentOS 9 olmayacak. CentOS Linux 7 kullanıcılarının panik yapmasına gerek yok. CentOS 7, 2024'te ömrünün sonuna kadar devam edecek.


##### CentOS Linux 8'den CentOS Stream 8'e Geçiş

Komutu kullanarak CentOS 8'i mevcut en son sürüme güncelleyin:
```
$ sudo dnf update
```
###### dnf yükleme işleminde hata alırsak ne yapmalıyız.

```
[root@youtube ~]# sudo dnf install centos-release-stream
CentOS Linux 8 - AppStream                                                                                                                                                                                   163  B/s |  38  B     00:00
Error: Failed to download metadata for repo 'appstream': Cannot prepare internal mirrorlist: No URLs in mirrorlist
[root@youtube ~]# sudo sed -i -e "s|mirrorlist=|#mirrorlist=|g" /etc/yum.repos.d/CentOS-*
[root@youtube ~]# sudo sed -i -e "s|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g" /etc/yum.repos.d/CentOS-*

After updating the system, reboot it. Check the current CentOS 8 version using command:
```

```
$ cat /etc/redhat-release 
CentOS Linux release 8.3.2011
```

Ardından, aşağıdaki komutu kullanarak CentOS Stream deposunu etkinleştirin:
```
$ sudo dnf install centos-release-stream
```
###### Örnek çıktı:
```
Last metadata expiration check: 0:35:27 ago on Wednesday 09 December 2020 12:44:07 PM IST.
Dependencies resolved.
=========================================================================
 Package                 Arch     Version                 Repo      Size
=========================================================================
Installing:
 centos-release-stream   x86_64   8.1-1.1911.0.7.el8      extras    11 k

Transaction Summary
=========================================================================
Install  1 Package

Total download size: 11 k
Installed size: 6.6 k
Is this ok [y/N]: y
Downloading Packages:
centos-release-stream-8.1-1.1911.0.7.el8  17 kB/s |  11 kB     00:00    
-------------------------------------------------------------------------
Total                                    5.9 kB/s |  11 kB     00:01     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                 1/1 
  Installing       : centos-release-stream-8.1-1.1911.0.7.el8.x86_   1/1 
  Verifying        : centos-release-stream-8.1-1.1911.0.7.el8.x86_   1/1 

Installed:
  centos-release-stream-8.1-1.1911.0.7.el8.x86_64                        

Complete!
```

Mevcut tüm CentOS Linux depolarını CentOS Stream depolarıyla değiştirin:
```
$ sudo dnf swap centos-{linux,stream}-repos
```
Son olarak, CentOS Linux 8'i CentOS Stream 8'e geçirmek için aşağıdaki komutu çalıştırın:
```
$ sudo dnf distro-sync
```
Distro-sync komutu, gerekli yükseltmeleri, düşürmeleri yapacak veya seçili yüklü paketleri, etkinleştirilmiş herhangi bir depoda bulunan en son sürümle eşleşecek şekilde tutacaktır. Herhangi bir paket verilmezse, kurulu tüm paketler dikkate alınır. CentOS Stream 8'e geçişi başlatmak için Y yazın ve ENTER'a basın:


###### Örnek çıktı:

```
CentOS-Stream - AppStream                                                                               521 kB/s | 6.3 MB     00:12    
CentOS-Stream - Base                                                                                    304 kB/s | 2.3 MB     00:07    
CentOS-Stream - Extras                                                                                  5.1 kB/s | 7.0 kB     00:01    
Last metadata expiration check: 0:00:01 ago on Wednesday 09 December 2020 01:22:28 PM IST.
Dependencies resolved.
========================================================================================================================================
 Package                                         Architecture    Version                                Repository                 Size
========================================================================================================================================
Installing:
 centos-stream-release                           noarch          8.4-1.el8                              Stream-BaseOS              21 k
     replacing  centos-linux-release.noarch 8.3-1.2011.el8
     replacing  centos-release-stream.x86_64 8.1-1.1911.0.7.el8
Upgrading:
 NetworkManager                                  x86_64          1:1.30.0-0.2.el8                       Stream-BaseOS             2.5 M
 NetworkManager-libnm                            x86_64          1:1.30.0-0.2.el8                       Stream-BaseOS             1.8 M
 NetworkManager-team                             x86_64          1:1.30.0-0.2.el8                       Stream-BaseOS             142 k
 NetworkManager-tui                              x86_64          1:1.30.0-0.2.el8                       Stream-BaseOS             322 k
 avahi-glib                                      x86_64          0.7-20.el8                             Stream-BaseOS              14 k
 avahi-libs                                      x86_64          0.7-20.el8                             Stream-BaseOS              62 k
 bind-export-libs                                x86_64          32:9.11.20-6.el8                       
.
.
.
.
baseos                     57 k
 python3-subscription-manager-rhsm               x86_64          1.28.5-1.el8                           Stream-BaseOS             362 k
 subscription-manager                            x86_64          1.28.5-1.el8                           Stream-BaseOS             1.1 M
 subscription-manager-rhsm-certificates          x86_64          1.28.5-1.el8                           Stream-BaseOS             258 k
 usermode                                        x86_64          1.113-1.el8                            baseos                    202 k

Transaction Summary
========================================================================================================================================
Install    9 Packages
Upgrade  107 Packages

Total download size: 205 M
Is this ok [y/N]: y
```

İnternet hızınıza bağlı olarak bu biraz zaman alacaktır. CentOS Stream 8 geçişi tamamlandıktan sonra doğrulamak için aşağıdaki komutu çalıştırın:
```
$ cat /etc/redhat-release 
CentOS Stream release 8
```



