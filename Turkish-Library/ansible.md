## CentOS/RHEL 8'de Ansible Otomasyon Aracı Nasıl Kurulur

Ansible, sistem yöneticilerinin düğümlere herhangi bir aracı yüklemeye gerek kalmadan yüzlerce düğümü merkezi bir sunucudan yapılandırmasına ve kontrol etmesine olanak tanıyan ücretsiz ve açık kaynaklı bir otomasyon aracıdır.

Uzak düğümlerle iletişim kurmak için SSH protokolüne güvenir. Puppet ve Chef gibi diğer yönetim araçlarıyla karşılaştırıldığında Ansible, kullanım ve kurulum kolaylığı nedeniyle favori olarak karşımıza çıkıyor.

Bu eğitimde, Ansible otomasyon aracını RHEL/CentOS 8 Linux dağıtımına nasıl kuracağınızı ve yapılandıracağınızı öğreneceksiniz.

ÖNEMLİ: CentOS 8 için, ansible geleneksel olarak EPEL deposu aracılığıyla dağıtıldı, ancak henüz resmi bir paket yok, ancak üzerinde çalışılıyor. Bu nedenle, Ansible'ı CentOS 8'e kurmak için standart PIP (Python paket yöneticisi) kullanıyoruz.

RHEL 8'de, bu makalede gösterildiği gibi yüklemek istediğiniz ilgili Ansible sürümü için resmi Red Hat deposunu etkinleştirin. RHEL 8'DE PIP KULLANMAYIN!.

##### Adım 1: Python3'ü Yükleme


Genellikle, RHEL 8 ve CentOS 8, varsayılan olarak önceden kurulmuş Python3 ile birlikte gelir. Ancak, herhangi bir nedenle Python3 kurulu değilse, aşağıdaki dnf komutlarını kullanarak kurun. Sudo ayrıcalıklarıyla normal kullanıcı olarak oturum açmanız gerektiğinden emin olun.
```
# su - ravisaive
$ sudo dnf update
$ sudo dnf install python3
```
Python3'ü RHEL ve CentOS 8'e yükleyin
Python3'ü RHEL ve CentOS 8'e yükleyin
Gerçekten python3'ün kurulu olduğunu doğrulamak için komutu çalıştırın.
```
$ python3 -V
```
Python Sürümünü Doğrulayın
Python Sürümünü Doğrulayın
##### 2. Adım: PIP Kurulumu – Python Paket Yükleyici
Pip, Python'un önceden kurulu olarak gelen paket yöneticisidir, ancak yine, sisteminizde Pip'in eksik olması durumunda, komutu kullanarak kurun.
```
$ sudo dnf install python3-pip
```
PIP'yi CentOS ve RHEL 8'e yükleyin
PIP'yi CentOS ve RHEL 8'e yükleyin
##### Adım 3: Ansible Otomasyon Aracını Yükleme
Tüm ön koşullar karşılandığında, CentOS 8'de komutu çalıştırarak ansible'ı kurun.
```
# pip3 install ansible --user
```
Ansible'ı CentOS ve RHEL 8'e yükleyin
Ansible'ı CentOS ve RHEL 8'e yükleyin
RHEL 8'de, ilgili Ansible sürümünü gösterildiği gibi kurmak için Ansible Engine deposunu etkinleştirin,

##### abonelik yöneticisi depoları --ansible-2.8-for-rhel-8-x86_64-rpms'yi etkinleştirin

```
dnf -y install ansible
```
Ansible sürümünü kontrol etmek için çalıştırın.
```
# ansible --version
```
Ansible Kurulumunu Kontrol Edin
Ansible Kurulumunu Kontrol Edin
Mükemmel! Gördüğünüz gibi Ansible'ın kurulu versiyonu Ansible 2.8.5.

##### 4. Adım: Ansible Otomasyon Aracını Test Etme
Ansible'ı test etmek için önce ssh'nin çalışır durumda olduğundan emin olun.
```
$ sudo systemctl status sshd
```
CentOS ve RHEL 8'de SSH Durumunu Kontrol Edin
CentOS ve RHEL 8'de SSH Durumunu Kontrol Edin
Ardından, ana makineleri tanımlamak için /etc/ansible dizininde hosts dosyasını oluşturmamız gerekiyor.
```
$ sudo mkdir /etc/ansible  
$ cd /etc/ansible
$ sudo touch hosts
```
Dosyanın ana bilgisayarları, tüm uzak düğümlerinize sahip olacağınız envanter olacaktır.

Şimdi hosts dosyasını favori düzenleyicinizle açın ve gösterildiği gibi uzak düğümü tanımlayın.
```
[web]
192.168.0.104
```
Ardından, ortak anahtarı uzak düğüme kopyalayacağımız SSH anahtarlarını oluşturun.
```
$ ssh-keygen
```
Ansible için SSH Anahtarları Oluşturun
Ansible için SSH Anahtarları Oluşturun
Oluşturulan SSH anahtarını uzak düğüme kopyalamak için komutu çalıştırın.
```
$ ssh-copy-id tecmint@192.168.0.104
```
SSH Anahtarını Uzak Linux'a Kopyalayın
SSH Anahtarını Uzak Linux'a Kopyalayın
Şimdi gösterildiği gibi uzak düğüme ping atmak için Ansible'ı kullanın.
```
$ ansible -i /etc/ansible/hosts web -m ping  
```
Ansible Ping Uzak Ana Bilgisayar
Ansible Ping Uzak Ana Bilgisayar
Ansible'ı RHEL/CentOS 8 Linux dağıtımına başarıyla kurmayı ve test etmeyi başardık. Herhangi bir sorunuz varsa, aşağıdaki yorumlar bölümünde bizimle paylaşın.