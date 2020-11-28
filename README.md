# Jarkom_Modul3_Lapres_T04

## 1. Membuat Topologi

![gambar](https://user-images.githubusercontent.com/55182321/100518547-cb5aab00-31c4-11eb-907b-e6d375f3bea1.png)

Surabaya sebagai router dihubungkan dengan 3 switch yang mana:
Switch 1 Menghubungkan ke 2 client yaitu gresik dan Sidoarjo dengan interface eth1
Switch 2 Menghubungkan ke 3 server yaitu Malang Tuban Mojokerto dengan interface eth3
Switch 3 Menghubungkan ke 2 client yaitu Banyuwangi Madiun dengan interface eth2


Sehingga topologi yang dibuat di PuTTY seperti:
![gambar](https://user-images.githubusercontent.com/55182321/100518551-d9103080-31c4-11eb-99d1-f963091c1f8c.png)

## 2. Membuat Surabaya Menjadi DHCP Relay

Agar bisa membuat SURABAYA menajdi dhcp relay, harus mengatur dhcp server di TUBAN terlebih dahulu

Mengetikan nano /etc/default/isc-dchp-server pada TUBAN

![gambar](https://user-images.githubusercontent.com/55182321/100518929-41f8a800-31c7-11eb-9288-0359ee027a6c.png)

Lalu, me relay dengan SURABAYA dengan mengetikan nano /etc/default/isc-dchp-relay

![gambar](https://user-images.githubusercontent.com/55182321/100519007-c4816780-31c7-11eb-862e-b6de94ded614.png)

## -----dhcpd.conf-----

![gambar](https://user-images.githubusercontent.com/55182321/100519033-eaa70780-31c7-11eb-82ca-465ff680eaf8.png)
![gambar](https://user-images.githubusercontent.com/55182321/100519274-85541600-31c9-11eb-928a-4257b3735c61.png)



## 3. Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200

Mengetikan range pada pada yang mengarah ke subnet 192.168.0.0 dan hasilnya di cek di client: 

![gambar](https://user-images.githubusercontent.com/55182321/100519102-67d27c80-31c8-11eb-8d1b-c3258352787d.png)
![gambar](https://user-images.githubusercontent.com/55182321/100519105-6b660380-31c8-11eb-9739-80226b089ad7.png)

## 4. Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70

Mengetikan range pada pada yang mengarah ke subnet 192.168.1.0 dan hasilnya di cek di client:

![gambar](https://user-images.githubusercontent.com/55182321/100519472-a9642700-31ca-11eb-9584-3d0d9a1f48d0.png)
![gambar](https://user-images.githubusercontent.com/55182321/100519477-ac5f1780-31ca-11eb-8e75-5b14f0dfca43.png)

## 5. Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP

Mengetikan Nama DNS seperti di dhcpd.conf dan 

Mengecek pada setiap client menggunakan cat /etc/resolv.conf

![gambar](https://user-images.githubusercontent.com/55182321/100519529-0fe94500-31cb-11eb-8e08-8721ef2016b2.png)

## 6. Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan client pada subnet 3 mendapatkan peminjaman IP selama 10 menit.

Mengatur di dhcpd.conf default-lease time untuk subnet 1 menjadi 300 (detik) dan subnet 3 menjadi 600 (detik)

## -----squid.conf----- 

![gambar](https://user-images.githubusercontent.com/55182321/100520363-044c4d00-31d0-11eb-8a3f-dad40b942879.png)
![gambar](https://user-images.githubusercontent.com/55182321/100520369-08786a80-31d0-11eb-8f21-c6ecb75bb013.png)


## 7. User autentikasi 

Mengatur pada squid.conf seperti dibawah nomer #7 

Jika di aktifkan maka di web akan muncul seperti:

![gambar](https://user-images.githubusercontent.com/55182321/100519933-9ef75c80-31cd-11eb-9d7b-a78b2f469449.png)

## 8.setiap hari Selasa-Rabu pukul 13.00-18.00 bisa mengakses proxy

Membuat file di /etc/squid/acl.conf seperti

![gambar](https://user-images.githubusercontent.com/55182321/100520481-70c74c00-31d0-11eb-9617-27bfac1f1416.png)

Untuk nomer 8 ada di baris pertama diperbolehkan hari selasa dan rabu maka hanya ada "TW" 

## 9. setiap hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya bisa mengakses proxy

Sama seperti nomer 8 

![gambar](https://user-images.githubusercontent.com/55182321/100520481-70c74c00-31d0-11eb-9617-27bfac1f1416.png)

Untuk nomer 9 ada pada baris 2 dan 3. Pada baris 2 menggunakan TWH (selasa rabu kamis) dari jam 21:00 sampai 23:59, dan baris 3 menggunkan WHF (rabu kamis jumat) dari jam 00:00 sampai jam 09:00

## 10. setiap mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id

Mengetikan seperti pada konfigurasi squid.conf dibawah #10


"acl NONEED dstdomain goole.com " membuat acl NONEED dengan tujuanya google.com
"deny_info http://monta.if.its.ac.id" mengarahkan ke monta.if.its.ac.id
"http_reply_access deny NONEED" Menandakan deny info hanya ditujukan ke acl NONEEED

## 11. Mengubah error page default squid

Mendowload menggunakan `wget 10.151.36.202/ERR_ACCESS_DENIED` dan memindah ke folder /usr/share/squid/errors/English dengan nama yang sama 

![gambar](https://user-images.githubusercontent.com/55182321/100521036-ca7d4580-31d3-11eb-9cfd-5974ea174bcc.png)

dan memasukan pada squid.conf 

"deny_info ERR_ACCESS_DENIED all" mengarahkan error ke gambar ERR_ACCESS_DENIED

![gambar](https://user-images.githubusercontent.com/55182321/100521251-ed5c2980-31d4-11eb-9a99-311fc126ee1f.png)

## 12. menggunakan proxy cukup dengan mengetikkan domain janganlupa-ta.t04.pw dan memasukkan port 8080. 

Membuat Domain di Malang 
![gambar](https://user-images.githubusercontent.com/55182321/100521141-6d35c400-31d4-11eb-9ec1-639baff3737e.png)

Mengetikan janganlupa-ta.t04.pw dan dengan ip mojokerto

![gambar](https://user-images.githubusercontent.com/55182321/100521144-71fa7800-31d4-11eb-84bd-9b2594354a4d.png)

