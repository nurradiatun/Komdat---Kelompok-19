# Komdat-Kelompok-19

[Tentang](#Tentang) | [Instalasi](#Instalasi) | [Konfigurasi](#Konfigurasi) | [Otomatisasi](#Otomatisasi) | [Cara Pemakaian](#Cara-Pemakaian) | [Pembahasan](#Pembahasan) | [Referensi](#referensi)
:---:|:---:|:---:|:---:|:---:|:---:|:---:

## Tentang

## Instalasi

### Kebutuhan
- Apache 2.X / Nginx
- PHP 5.3.7 (or later) with php-mcrypt & GD enabled [PHP5.5+ recommended]
- MySQL 5.x+

### Cara Instalasi
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

6. 


