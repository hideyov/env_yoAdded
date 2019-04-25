# vagrant 混乱中

## 190423の現状

### 1) MyCentOS_default_1556031688011_69720

host側：$ MyVagrant/MyCentOS

guest側：MyCentOS_default

Vagrantfile //

config.vm.box = "bento/centos-6.8"

config.vm.network "private_network", ip: "192.168.33.10"

/# config.vm.synced_folder "../data", "/vagrant_data" (設定なし)

同期 //

host側：

guest側：

webサーバー //

[vagrant@localhost php_lessons]$ php -S 192.168.33.10:8000 => PHPが用意しているウェブサーバーが起動

### 2) myCentOSVM_default_1556025432413_78271

host側：$ MyVagrant/myCentOSVM

guest側：MyCentOSMV_default

Vagrantfile // 

config.vm.box = "centos64"

config.vm.network "private_network", ip: "192.168.33.11"

/# config.vm.synced_folder "../data", "/vagrant_data" // 設定なし

同期 //

host側：~/MyVagrant/myCentOSVM

guest側：/vagrant

同期している（guest側からVagrantfileにアクセスできるし、ファイル操作も同期している）

### 3) myCentOSVM_2_default

host側：$ MyVagrant/myCentOSVM_2

guest側：MyCentOSMV_2_default

Vagrantfile // 

config.vm.box = "centos64"

config.vm.network "private_network", ip: "192.168.33.12"

/# config.vm.synced_folder "../data", "/vagrant_data" // 設定なし

同期 //

host側：~/MyVagrant/myCentOSVM_2

guest側：/vagrant

同期している（guest側からVagrantfileにアクセスできるし、ファイル操作も同期している）

Provision //

config.vm.provision "shell", :path => "provision.sh"

provision.sh ::::::::::

sudo yum -y install httpd

sudo service httpd start

sudo chkconfig httpd on

::::::::::::::::::::::::

[??? question] またもや ERR_EMPTY_RESPONSE // webサーバーが起動しているにもかかわらず、ブラウザに表示されない。