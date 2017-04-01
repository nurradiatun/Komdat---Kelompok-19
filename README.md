# Komdat-Kelompok-19
<h1 align="center"><img src="https://github.com/nurradiatun/Komdat-Kelompok-19/blob/master/logo.png"></h1>

[Tentang](#web-paste) | [Instalasi](#instalasi-web-server) | [Konfigurasi](#konfigurasi) | [Maintenance](#maintenance) | [Cara Pemakaian](#cara-pemakaian) | [Pembahasan](#pembahasan) | [Referensi](#referensi)
:---:|:---:|:---:|:---:|:---:|:---:|:---:

## Aplikasi Web Paste
Paste merupakan aplikasi web *pastebins* yang dapat digunakan untuk memasukkan *source code* dan teks debugging umum. Fitur yang tersedia pada aplikasi ini yaitu mengedit, menghapus, serta mencari kata pada hasil Paste. Fitur tersebut dapat dijalankan dengan melakukan *login* terlebih dahulu.

## Instalasi Web Server
### Membuat VM Ubuntu Server
Install Ubuntu Server 16.04 menggunakan *Virtual Disk Manager*, kemudian buat VM baru pada Virtual Box dengan nama *ubuntu* dan tipe *Ubuntu 64-bit*. Jalankan VM Ubuntu Server tersebut. Setting username ``` student ```  dan password ``` student ```.

### Setting *port-forwading* VM
Tujuan dilakukan setting *port-forwading* agar VM dapat diakses dari luar melalui alamat IP *host(localhost)* . 
Masuk ke ``` Setting -> Network -> Advanced -> Port-Forwading ``` dan tambahkan dua aturan berikut :

| Name  	| Protocol | Host IP | Host Port | Guest IP | Guest Port |
| -------- 	| :---------: | :--------: | :----------: | :---------: | :------------: |
| http     	| TCP	| 	       | 8888	 | 		 | 80		     |
| ssh    	| TCP	| 	       | 2222	 | 		 | 22		     |

Dengan demikian , jika kita mengakses ``` localhost:8888 ``` di host, maka akan diteruskan di *guest* VM.

### Kebutuhan
- Apache 2.X / Nginx
- PHP 5.3.7 (or later) with php-mcrypt & GD enabled [PHP5.5+ recommended]
- MySQL 5.x+

### Instalasi LAMP(Linux Apache MySQL PHP)
```
#install SSH
sudo apt update
sudo apt install ssh
```
Setelah terinstall SSH, kita dapat mengakses VM secara *remote*. Buka terminal di *host* untuk login *remote* ke *port* 2222.

```
#akses remote dari host
ssh student @localhost -p 2222

#install Apache, MySQL, PHP
sudo apt install apache2
sudo apt install mysql-server
sudo apt install php
sudo apt install libapache2-mod-php
sudo apt install php-mysql
sudo apt install php-gd php-mcrypt php-mbstring php-xml php-ssh2
sudo service apache2 restart
```
Cek instalasi Apache dengan membuka laman http://localhost:8888 jika terdapat tulisan It's Work! maka instalasi LAMP berhasil
### Instalasi Aplikasi Web Paste
1. Masuk ke folder ``` /var/www/html	```
```
$ cd /var/www/html
```

2. Unduh arsip instalasi Paste dan simpan ke dalam direktori PASTE
```
$ sudo git clone https://github.com/jordansamuel/PASTE.git
$ cd PASTE
```

3. Melakukan cek instalasi pada file config.php , tmpt/temp.tdata , sitemap.xml
```
$ sudo chmod 777 config.php
$ sudo chmod 777 tmp/temp.tdata
$ sudo chmod 777 sitemap.xml
```

4. Masuk ke browser untuk melakukan pengecekan status file config.php , tmpt/temp.tdata , sitemap.xml
```
localhost:8888/PASTE/install
```

5. Masuk ke folder docs, instal phpmyadmin dan buat database, kemudian pindahkan file config.example.php menjadi file config.php
```
$ cd docs
$ sudo apt-get install phpmyadmin
$ mysql -u root -p
mysql> CREATE DATABASE paste;
$ sudo mv config.example.php config.php
```

6. Masuk ke localhost (localhost -> alamat IP atau URL kamu). Selanjutkan langkah-langkah konfigurasi:
	- Konfigurasi Database
	![](https://github.com/nurradiatun/Komdat-Kelompok-19/blob/master/cek%20install.png)
    
    - Konfigurasi Admin Account
    ![](https://github.com/nurradiatun/Komdat-Kelompok-19/blob/master/admin.png)

	- setelah itu lakukan penghapusan folder instalasi pada terminal, masuk ke directori var/www/html/PASTE 
	```
    $ rm -rf install/
    ```
    
    - Kembali ke localhost tadi, refresh, maka muncul login admin
    ![](https://github.com/nurradiatun/Komdat-Kelompok-19/blob/master/login%20adminn.png)
    
    - Selesai (Tampilan dashboard Admin)
    ![](https://github.com/nurradiatun/Komdat-Kelompok-19/blob/master/dashboard.png)
    
## Konfigurasi
Pastikan pada konfigurasi Apache, module mod_rewrite sudah dalam keadaan diaktifkan. Untuk melihat module sudah aktif, ketikkan perintah ``` nano config.php``` kemudian set ``` mod_rewrite ="1"; ```
Sekarang paste hanya mendukung hingga 4GB, dan atur konfigurasi pada config.php. Namun, ini bergantung pada nilai pos_max_size pada file PHP configurasi anda.
```
// Max paste size in MB. This value should always be below the value of
// post_max_size in your PHP configuration settings (php.ini) or empty errors will occur.
// The value we got on installation of Paste was: post_max_size = 1G
// Otherwise, the maximum value that can be set is 4000 (4GB)
$pastelimit = "1"; // 0.5 = 512 kilobytes, 1 = 1MB
```
Untuk mengaktifkan registrasi dengan OAUTH lihat block dibawah ini pada config.php
```
// OAUTH (to enable, change to yes and edit)
$enablefb = "no";
$enablegoog = "no";

// "CHANGE THIS" = Replace with your details
// Facebook
define('FB_APP_ID', 'CHANGE THIS'); // Your application ID, see https://developers.facebook.com/docs/apps/register
define('FB_APP_SECRET', 'CHANGE THIS');    // What's your Secret key

// Google 
define('G_Client_ID', 'CHANGE THIS'); // Get a Client ID from https://console.developers.google.com/projectselector/apis/library
define('G_Client_Secret', 'CHANGE THIS'); // What's your Secret ID
define('G_Redirect_Uri', 'http://urltoyour/installation/oauth/google.php'); // Leave this as is
define('G_Application_Name', 'Paste'); // Make sure this matches the name of your application
```
Untuk menyimpan (CTRL+ O) atau (CTRL+X).
Untuk konfigurasi lainnya dapat diatur menggunakan Admin Panel

### Konfigurasi Admin Panel
Pada halaman dashboard admin terdapat menu konfigurasi, dimana admin dapat melakukan pengaturan pada web. 
1. Site Info
Admin dapat melakukan konfigurasi informasi site
![](https://github.com/nurradiatun/Komdat-Kelompok-19/blob/master/configurasi1.png)
2. Permission
![](https://github.com/nurradiatun/Komdat-Kelompok-19/blob/master/configurasi2.png)
3. Captcha Settings
![](https://github.com/nurradiatun/Komdat-Kelompok-19/blob/master/configure3.png)
4. Mail Settings
![](https://github.com/nurradiatun/Komdat-Kelompok-19/blob/master/configure4.png)

## Maintenance 
PASTE menyediakan fitur maintenance apabila admin web ingin melakukan perbaikan pada situs.
1. Pada halaman admin terdapat menu task, klik menu task.
![](https://github.com/nurradiatun/Komdat-Kelompok-19/blob/master/dashboard.png)
2. Kemudian muncul halaman maintenance 
![](https://github.com/nurradiatun/Komdat-Kelompok-19/blob/master/maintenance.png)
Pada halaman ini, admin dapat melakukan penghapusan pada file yang sudah expired, membersihkan history pada admin, menghapus akun yang tidak diverifikasi, dan menghapus semua hasil paste.

## Cara Pemakaian
1. Tampilan Aplikasi Web Paste
![](https://github.com/nurradiatun/Komdat-Kelompok-19/blob/master/masuk1.png)
2.


## Pembahasan
Terdapat banyak jenis aplikasi Pastebins diantaranya Pastebin dan Paste. Paste memiliki beberapa fitur, yaitu download, edit, embed yang digunakan untuk menambahkan paste pada web atau blog dengan *copy* *link* yang diberikan, dan *archive* untuk melihat hasil paste dari orang lain.
Tidak jauh berbeda dengan Paste, fitur Pastebin juga 