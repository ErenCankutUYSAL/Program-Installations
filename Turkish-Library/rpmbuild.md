## Linux RPM paketi nasıl oluşturulur

Dağıtmak istediğiniz harika bir komut dosyası yazdınız, neden onu bir RPM olarak paketlemiyorsunuz?

Bu makale, Linux sistemlerinizden kolay kurulum, güncelleme ve kaldırma için bir komut dosyasını bir RPM dosyasına nasıl paketleyeceğinizi gösterir. Ayrıntılara geçmeden önce, bir RPM paketinin ne olduğunu ve nasıl kurabileceğinizi, sorgulayabileceğinizi, kaldırabileceğinizi ve en önemlisi kendiniz nasıl oluşturabileceğinizi açıklayacağım.

Bu makale şunları kapsar:

RPM paketi nedir?
RPM paketi nasıl oluşturulur.
Bir RPM paketi nasıl kurulur, sorgulanır ve kaldırılır.

### RPM paketi nedir?

RPM, Red Hat Paket Yöneticisi anlamına gelir. Red Hat tarafından geliştirilmiştir ve öncelikle Red Hat tabanlı Linux işletim sistemlerinde (Fedora, CentOS, RHEL, vb.) kullanılır.

Bir RPM paketi .rpm uzantısını kullanır ve farklı dosyalardan oluşan bir pakettir (koleksiyondur). Aşağıdakileri içerebilir:

- Yürütülebilir dosyalar olarak da bilinen ikili dosyalar (nmap, stat, xattr, ssh, sshd vb.).
- Konfigürasyon dosyaları (sshd.conf, updatedb.conf, logrotate.conf, vb.).
- Dokümantasyon dosyaları (BENİ OKU, YAPILACAKLAR, YAZAR, vb.).

Bir RPM paketinin adı şu biçimdedir:
```
<name>-<version>-<release>.<arch>.rpm
```

Bir örnek:
```
bdsync-0.11.1-1.x86_64.rpm
```
Bazı paketler, oluşturuldukları dağıtımın kısa bir sürümünü de içerir, örneğin:
```
bdsync-0.11.1-1.el8.x86_64.rpm
```

### RPM paketi nasıl oluşturulur

Bir RPM paketi oluşturmak için aşağıdaki bileşenlere ihtiyacınız olacak:

- RHEL veya Fedora gibi RPM tabanlı bir dağıtım çalıştıran bir iş istasyonu veya sanal makine.
- Paketi oluşturmak için yazılım.
- Paketlenecek kaynak kodu.
- RPM'yi oluşturmak için SPEC dosyası.

### Gerekli yazılımı yükleme

RPM paketini oluşturmak için aşağıdaki paketlerin yüklenmesi gerekir:
```
sudo dnf install -y rpmdevtools rpmlint
```

rpmdevtools'u kurduktan sonra, RPM paketleri oluşturmak için ihtiyacınız olan dosya ağacını oluşturun:

```
$ rpmdev-setuptree
``` 

RPM paketlerini normal (kök değil) bir kullanıcı olarak oluşturursunuz, böylece derleme ortamınız ana dizininize yerleştirilir. Bu dizin yapısını içerir:

``` 
rpmbuild/
├── BUILD
├── RPMS
├── SOURCES
├── SPECS
└── SRPMS
``` 

- BUILD dizini, RPM paketinin oluşturma işlemi sırasında kullanılır. Burası geçici dosyaların depolandığı, taşındığı vb. yerdir.
- RPMS dizini, .spec dosyasında veya derleme sırasında belirtilmişse, farklı mimariler ve noarch için oluşturulmuş RPM paketlerini içerir.
- SOURCES dizini adından da anlaşılacağı gibi kaynakları içerir. Bu basit bir betik, derlenmesi gereken karmaşık bir C projesi, önceden derlenmiş bir program vb. olabilir. Genellikle kaynaklar .tar.gz veya .tgz dosyaları olarak sıkıştırılır.
- SPEC dizini, .spec dosyalarını içerir. .spec dosyası, bir paketin nasıl oluşturulacağını tanımlar. Daha sonra bunun hakkında.
- SRPMS dizini .src.rpm paketlerini tutar. Kaynak RPM paketi bir mimariye veya dağıtıma ait değildir. Gerçek .rpm paket yapısı, .src.rpm paketini temel alır.

Bir .src.rpm paketi çok esnektir, çünkü diğer tüm RPM tabanlı dağıtım ve mimarilerde oluşturulabilir ve yeniden oluşturulabilir.

Artık her dizinin ne içerdiğine aşinasınız, bu nedenle şimdi dağıtmak için basit bir komut dosyası oluşturun:

```
$ cat << EOF >> hello.sh
#!/bin/sh
echo "Hello world"
EOF
```

Bu, terminale "Merhaba dünya" yazan merhaba.sh adında bir kabuk betiği oluşturur. Çok basit, ancak ambalajı göstermek için yeterli.

### Komut dosyasını belirtilen dizine yerleştirin

Komut dosyanız için bir paket oluşturmak için, komut dosyanızı RPM oluşturma sisteminin içinde olmasını beklediği dizine koymalısınız. Çoğu projenin yaptığı gibi anlamsal sürüm oluşturmayı kullanarak bunun için bir dizin oluşturun ve ardından merhaba.sh'yi içine taşıyın:
```
$ mkdir hello-0.0.1
$ mv hello.sh hello-0.0.1
```

Çoğu kaynak kodu bir arşiv olarak dağıtılır, bu nedenle bir arşiv dosyası oluşturmak için tar komutunu kullanın:

```
$ tar --create --file hello-0.0.1.tar.gz hello-0.0.1
```

Ardından, az önce oluşturduğunuz tarball'ı SOURCES dizinine taşıyın:
```
$ mv hello-0.0.1.tar.gz SOURCES
```
### Bir .spec dosyası oluşturun

Bir RPM paketi, bir .spec dosyası tarafından tanımlanır. Bir .spec dosyasının sözdizimi katıdır, ancak rpmdev sizin için bir ortak dosya dosyası oluşturabilir:

```
$ rpmdev-newspec hello
```
Bu, SPECS dizinine taşımanız gereken merhaba.spec adlı bir dosya oluşturur. Dizin yapısının nasıl göründüğünü görmek için ~/rpmbuild ağacını çalıştırın:
```
/home/tux/rpmbuild/
├── BUILD
├── BUILDROOT
├── RPMS
├── SOURCES
│   └── hello-0.0.1.tar.gz
├── SPECS
│   └── hello.spec
└── SRPMS
```

Oluşturulan merhaba.spec dosyası iyi bir başlangıç noktası sağlar, ancak ne inşa ettiğiniz hakkında herhangi bir özel bilgiye sahip değildir. Oluşturulan .spec dosyası, yazılımı derleyip oluşturacağımı varsayar.

Bir Bash betiğini paketliyorsunuz, bu yüzden yapabileceğiniz bazı basitleştirmeler var. Örneğin, derlenecek kod olmadığı için Derleme işlemi yoktur. BuildArch: noarch'ı ekledim çünkü bu paket 32-bit, 64-bit, Arm ve Bash çalıştıran diğer CPU mimarisi için geçerlidir.

Paketin Bash'in kurulu olmasını sağlaması için Requires: bash'ı da ekledim. Bu basit "merhaba dünya" betiği elbette herhangi bir kabukta çalışır, ancak bu tüm betikler için geçerli değildir, dolayısıyla bu, bağımlılıkları göstermenin iyi bir yoludur.

```
Name:           hello
Version:        0.0.1
Release:        1%{?dist}
Summary:        A simple hello world script
BuildArch:      noarch

License:        GPL
Source0:        %{name}-%{version}.tar.gz

Requires:       bash

%description
A demo RPM build

%prep
%setup -q

%install
rm -rf $RPM_BUILD_ROOT
mkdir -p $RPM_BUILD_ROOT/%{_bindir}
cp %{name}.sh $RPM_BUILD_ROOT/%{_bindir}

%clean
rm -rf $RPM_BUILD_ROOT

%files
%{_bindir}/%{name}.sh

%changelog
* Sun Nov  18 2020 Valentin Bajrami <valentin.bajrami@slimmer.ai> - 0.0.1
- First version being packaged
```

Anlayabileceğiniz gibi, .spec dosyalarında birçok kısayol vardır. Bunlara makro denir ve Fedora paketleme belgelerinde bulunanların mükemmel bir listesi vardır. .spec dosyalarınızda makro kullanmanız önemlidir. Tüm RPM sistemlerinde tutarlılığı sağlamaya yardımcı olurlar, dosya adlarında ve sürüm numaralandırmasında hatalardan kaçınmanıza yardımcı olurlar ve komut dosyanızın yeni bir sürümünü yayınladığınızda .spec dosyasını güncellemeyi kolaylaştırırlar.

Örneğin, %files bölümünün altında tam olarak hangi dosyaların yüklendiğini belirtmeniz gerekir. Burada açıkça aşağıdaki satırı koydum:
```
%files
%{_bindir}/%{name}.sh
```
Bu, betiğin %{_bindir} (varsayılan olarak /usr/bin'e çeviren, ancak kullanıcılar /usr/local/bin gibi farklı bir konuma yüklemek istediğinde yapılandırılabilen bir makro) konumuna gitmesini istediğim için işe yarıyor. ). Aşağıdakileri çalıştırarak makro değerlerini doğrulayabilirsiniz:
```
$ rpm --eval '%{_bindir}'
/usr/bin
```
Diğer faydalı makrolar:

- paketin %{name} adı (Ad: alanında tanımlandığı gibi)
- paketin %{version} sürümü (Sürüm: alanında tanımlandığı gibi)
- %{_datadir} paylaşılan veri (varsayılan olarak/usr/sbin)
- %{_sysconfdir} yapılandırma dizini (varsayılan olarak/etc)

### Hata durumunda .spec dosyasını kontrol etme (rpmlint)
rpmlint komutu, .spec dosyalarındaki hataları bulabilir:
```
$ rpmlint ~/rpmbuild/SPECS/hello.spec
SPECS/hello.spec: W: no-%build-section
SPECS/hello.spec: W: invalid-url Source0: hello-0.0.1.tar.gz
0 packages and 1 specfiles checked; 0 errors, 2 warnings.
```

Bildirilen 2 hata var, ancak ikisi de kabul edilebilir. Oluşturulacak kod yoktur, bu nedenle %build bölümüne gerek yoktur ve kaynak arşiv yerel bir dosyadır ve ağ URL'si yoktur.

Her şey iyi gözüküyor.

### Paketi oluşturma (rpmbuild)
RPM paketini oluşturmak için rpmbuild komutunu kullanın. Bu öğreticide daha önce .src.rpm (Kaynak RPM paketi) ile .rpm paketi arasındaki farktan bahsetmiştim.

.src rpm paketini oluşturmak için:
```
$ rpmbuild -bs ~/rpmbuild/SPECS/hello.spec
```
`-bs` bayrakları aşağıdaki anlamlara sahiptir:
```
-b: build
-s: source
```
İkili .rpm paketini oluşturmak için:
```
$ rpmbuild -bb ~/rpmbuild/SPECS/rm-ssh-offendingkey.spec
```
`-bb` bayrakları aşağıdaki anlamlara sahiptir:
```
-b: build
-b: binary
```
Her ikisini de oluşturmak için -ba kullanın.

Derleme işlemi tamamlandıktan sonra aşağıdaki dizin yapısına sahip olursunuz:
```
$ tree ~/rpmbuild/
/home/tux/rpmbuild/
├── BUILD
│   └── hello-0.0.1
│       ├── hello.sh
├── BUILDROOT
├── RPMS
│   └── noarch
│       └── hello-0.0.1-1.el8.noarch.rpm
├── SOURCES
│   └── hello-0.0.1.tar.gz
├── SPECS
│   └── hello.spec
└── SRPMS
```

### RPM paketini yükleme

Paketin başarılı bir şekilde oluşturulmasından sonra, dnf komutunu kullanarak RPM paketini kurabilirsiniz:
```
$ sudo dnf install ~/rpmbuild/RPMS/noarch/hello-0.0.1-1.el8.noarch.rpm
```
Doğrudan rpm komutuyla dönüşümlü olarak kurulabilir:
```
$ sudo rpm -ivh ~/rpmbuild/RPMS/noarch/hello-0.0.1-1.el8.noarch.rpm
```

### Paketin yüklendiğini doğrulayın

Paketin doğru şekilde kurulduğunu doğrulamak için aşağıdaki komutu çalıştırın:

```
$ rpm -qi hello
Name        : hello
Version     : 0.0.1
Release     : 1
Architecture: noarch
Install Date: Mon 09 Nov 2020 01:29:51 AM CET
Group       : Unspecified
Size        : 294
License     : GPL
Signature   : (none)
Source RPM  : hello-0.0.1-1.el8.src.rpm
Build Date  : Mon 09 Nov 2020 01:08:14 AM CET
Build Host  : slimmerAI
Summary     : A simple hello world script
Description : A demo RPM build
The %changelog entry of a package can be viewed, too:

$ rpm -q hello --changelog
* Sun Nov 08 2020 Valentin Bajrami <valentin.bajrami@slimmer.ai> - 0.1
- First version being packaged
```

### See what’s in the RPM package


It's easy to see which files an RPM package contains:
```
$ rpm -ql hello
/usr/bin/hello.sh
```

### RPM paketini kaldırma
Paketi sistemden kaldırmak, kurmak kadar kolaydır. dnf komutunu kullanabilirsiniz:
```
$ sudo dnf remove hello
```
Veya doğrudan rpm komutu:
```
$ sudo rpm --verbose --erase hello
```










