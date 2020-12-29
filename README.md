# Jarkom_Modul5_Lapres_E11

## Persiapan
Sebagai persiapan, pertama-tama pada PuTTY digunakan ```rm (namaUML) (namaUML) (dst)``` untuk menghapus file-file UML sebelumnya.\
Pada soal, diminta membuat topologi sebagai berikut:
![saol](https://github.com/arshana40/Jarkom_Modul5_Lapres_E11/blob/main/Screenshoot/Topologi%20soal.png)\
Lakukan ```nano topologi.sh``` untuk menyiapkan UML sesuai gambar, isikan:
```
# Switch
uml_switch -unix switch0 > /dev/null < /dev/null &
uml_switch -unix switch1 > /dev/null < /dev/null &
uml_switch -unix switch2 > /dev/null < /dev/null &
uml_switch -unix switch3 > /dev/null < /dev/null &
uml_switch -unix switch4 > /dev/null < /dev/null &
uml_switch -unix switch5 > /dev/null < /dev/null &

# Router
xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,10.151.70.49 eth1=daemon,,,switch0 eth2=daemon,,,switch3 mem=96M &
xterm -T BATU -e linux ubd0=BATU,jarkom umid=BATU eth0=daemon,,,switch3 eth1=daemon,,,switch2 eth2=daemon,,,switch5 mem=96M &
xterm -T KEDIRI -e linux ubd0=KEDIRI,jarkom umid=KEDIRI eth0=daemon,,,switch0 eth1=daemon,,,switch1 eth2=daemon,,,switch4 mem=96M &

# Server
xterm -T PROBOLINGGO -e linux ubd0=PROBOLINGGO,jarkom umid=PROBOLINGGO eth0=daemon,,,switch1 mem=128M &
xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN eth0=daemon,,,switch1 mem=128M &
xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=128M &
xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=128M &

# Klien 1
xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch4 mem=96M &
xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch5 mem=96M &

```
Jalankan UML dengan ```bash topologi.sh```, lalu lakukan penyetingan pada tiap-tiap UML.
Metode subnetting yang digunakan adalah VLSM, dengan hasil tree sebagai berikut:
![gambartree](https://github.com/arshana40/Jarkom_Modul5_Lapres_E11/blob/main/Screenshoot/tree.jpg)\
Setelah penyetingan selesai dilakukan, pastikan semua UML telah berfungsi dengan cara melakukan ping terhadap google

## Soal 1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi SURABAYA menggunakan iptables, namun Bibah tidak ingin kalian menggunakan MASQUERADE.\
Pada SURABAYA, buat file baru dengan ```nano 1.sh``` yang berisi jawaban dari no.1\
![gambar1](https://github.com/arshana40/Jarkom_Modul5_Lapres_E11/blob/main/Screenshoot/1.jpg)\
Digunakan /16 untuk berjaga-jaga apabila diperlukan ukuran dalam jumlah yang banyak.\
IP 10.151.70.50 adalah eth0 SURABAYA yang menuju internet\
Simpan, kemudian jalankan dengan ```bash 1.sh```.


## Soal 2
Kalian diminta untuk mendrop semua akses SSH dari luar Topologi (UML) Kalian pada server yang memiliki ip DMZ (DHCP dan DNS SERVER) pada SURABAYA demi menjaga keamanan.\
Pada SURABAYA, buat file baru dengan ```nano 2.sh``` yang berisi jawaban dari no.2\
![gambar](https://github.com/arshana40/Jarkom_Modul5_Lapres_E11/blob/main/Screenshoot/2.jpg)\
-d digunakan karena berfungsi mendefinisikan opsi alamat tujuan.\
10.151.71.96/29 adalah NID DMZ kelompok e11.\
Simpan, kemudian jalankan dengan ```bash 2.sh```.

## Soal 3
Karena tim kalian maksimal terdiri dari 3 orang, Bibah meminta kalian untuk membatasi DHCP dan DNS server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan yang berasal dari mana saja menggunakan iptables pada masing masing server , selebihnya akan di DROP.\
Pada MALANG dan MOJOKERTO, buat file baru dengan ```nano 3.sh``` yang berisi konfigurasi jawaban untuk no.3\
![gambar](https://github.com/arshana40/Jarkom_Modul5_Lapres_E11/blob/main/Screenshoot/3.jpg)\
```--connlimit-above 3``` digunakan karena soal meminta maksimal 3 buah koneksi.\
Simpan, kemudian jalankan pada ke-2 UML dengan ```bash 3.sh```.\
Untuk pengetesan, lakukan pengetesan dengan cara melakukan ping terhadap MALANG atau MOJOKERTO. Hanya ada 3 UML yang dapat melakukan ping, sedangkan UML ke-4 akan gagal melakukan ping jika konfigurasi telah benar

## Soal
Diminta untuk membatasi akses ke MALANG yang berasal dari SUBNET SIDOARJO dan SUBNET GRESIK dengan peraturan sebagai berikut:
### Soal 4
Akses dari subnet SIDOARJO hanya diperbolehkan pada pukul 07.00 - 17.00 pada hari Senin sampai Jumat. Selain itu paket akan di REJECT.\
Pada MALANG, buat file baru dengan ```nano 4.sh``` yang berisi konfigurasi jawaban untuk no.4\
![gambar](https://github.com/arshana40/Jarkom_Modul5_Lapres_E11/blob/main/Screenshoot/4.jpg)\
Konfigurasi diisikan sesuai permintaan soal, seperti yang terdapat pada gambar.\
Simpan, lalu lakukan bash untuk menerapkan konfigurasi. Untuk pengujian, dapat digunakan ```date``` untuk mengubah tanggal saat hendak memeriksa konfigurasi.
### Soal 5
Akses dari subnet GRESIK hanya diperbolehkan pada pukul 17.00 hingga pukul 07.00 setiap harinya. Selain itu paket akan di REJECT.\
Pada MALANG, buat file baru dengan ```nano 5.sh``` yang berisi konfigurasi jawaban untuk no.5\
![gambar](https://github.com/arshana40/Jarkom_Modul5_Lapres_E11/blob/main/Screenshoot/5.jpg)\
Konfigurasi diisikan sesuai permintaan soal, seperti yang terdapat pada gambar.\
Simpan, lalu lakukan bash untuk menerapkan konfigurasi. Untuk pengujian, dapat digunakan ```date``` untuk mengubah tanggal saat hendak memeriksa konfigurasi.

## Soal 6
Bibah ingin SURABAYA disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada PROBOLINGGO port 80 dan MADIUN port 80.\
Pada SURABAYA, buat file baru dengan ```nano 6.sh``` yang berisi konfigurasi jawaban untuk no.6\
![gambar](https://github.com/arshana40/Jarkom_Modul5_Lapres_E11/blob/main/Screenshoot/6.jpg)\
```--dport 80``` digunakan karena soal meminta port 80.\
```--every 2``` digunakan karena soal meminta didistribusikan secara bergantuan
```10.151.0.10 dan 10.151.0.11``` adalah IP PROBOLINGGO dan MADIUN.\
Simpan, lalu lakukan bash untuk menerapkan konfigurasi.

## Soal 7
Bibah ingin agar semua paket didrop oleh firewall (dalam topologi) tercatat dalam log pada setiap UML yang memiliki aturan drop.\
Pada MALANG dan MOJOKERTO:\
![gambar](https://github.com/arshana40/Jarkom_Modul5_Lapres_E11/blob/main/Screenshoot/No7-MalangMojokerto.jpg)\
Pada SURABAYA:\
![gambar](https://github.com/arshana40/Jarkom_Modul5_Lapres_E11/blob/main/Screenshoot/7.jpg)\
Simpan, lalu lakukan bash untuk menerapkan konfigurasi.

# Kesulitan Selama Pengerjaan
- saat pengerjaan awal, terdapat kesalahan saat melakukan subnetting dengan CIDR, sehingga perlu revisi total karena tidak ada satu soalpun yang dapat terselesaikan
- saat pengerjaan awal, terdapat kesalahan saat membuat topologi yang terlambat disadari, seperti salah memasukkan IP untuk eth0 SURABAYA, menyebabkan tidak ada satu soalpun yang terselesaikan
- saat awal revisi, penyetingan IP dilakukan oleh praktikan yang berbeda dengan praktikan yang melakukan penghitungan VLSM, sehingga membuat praktikan yang satunya kebingungan untuk mengoreksi ketika ada kesalahan karena perbedaan gambaran lokasi subnet a1,a2,dll
- dll




