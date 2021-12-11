# Jarkom-Modul-5-A06-2021
```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
# Jarkom-Modul-4-A06-2021
Lapres Praktikum Jaringan Komputer 2021 - Modul 4

- Muh. Nur Fajrin Amiruddin (05111940000005)
- Rihan Farih Bunyamin (05111940000165)
- Lathifa Itqonina Mardiyati (05111940000176)

# Soal Praktikum
Pada soal diberikan topologi berikut :

![topologi_fix](https://user-images.githubusercontent.com/55240758/145660476-659736b6-c9c3-4872-a4e9-ca7595c34e1d.jpg)

Maka selanjutnya kami diminta untuk merancang topologi yang sama pada GNS3 (A). Dimana terdapat keterangan bahwa :  
- Doriki adalah DNS Server  
- Jipangu adalah DHCP Server  
- Maingate dan Jorge adalah Web Server  
- Jumlah Host pada Blueno adalah 100 host  
- Jumlah Host pada Cipher adalah 700 host  
- Jumlah Host pada Elena adalah 300 host  
- Jumlah Host pada Fukurou adalah 200 host  

Setelah itu kami diminta untuk menentukan pembagian subnetting dengan teknik VSLM atau CIDR yang telah dilakukan sebelumnya (B). Pada pembagian subnetting ini kami menggunakan teknik **VSLM** sebagai berikut :

## VSLM (Variable Length Subnet Masking) 
Perhitungan IP dapat dilakukan dengan menggunakan metode VLSM, dimana kita mengelompokkan setiap PC yang terhubung ke router. Apabila jumlah PC yang terhubung dengan router pada switch yang sama, lebih dari satu maka PC-PC tersebut akan dijadikakan satu kelompok.

**Langkah 1 :** Membuat topologi sesuai soal  

**Langkah 2 :** Menentukan jumlah alamat IP yang dibutuhkan oleh tiap subnet dan melakukan labelling netmask berdasarkan jumlah IP yang dibutuhkan.   

![Subnetting_fix](https://user-images.githubusercontent.com/55240758/145660431-678f8aa6-0fac-45a9-b25b-743e42dd2a08.png)


Dimana kita akan menjumlahkan usable IP atau host yang dimiliki oleh masing-masing PC, kemudian setiap PC yang terhubung dengan 1 router, maka jumlah hostnya akan ditambahkan dengan 1, dan apabila PC terhubung dengan 2 router, maka jumlah hostnya akan ditambahkan dengan 2, lalu untuk router yang berhubungan dengan router lain, jumlah hostnya adalah 2, karena dia terhubung dengan PC dan juga router lain.

Berdasarkan penjelasan diatas dan label yang telah diberikan pada topologi, maka dapat didapatkan data sebagai berikut :

| Subnet | Jumlah Usable IP (Jumlah Host)| Length | 
| --- | --- | --- | 
| A1 (Doriki + Jipangu → Water7) | 3 | /29 |
| A2 (Blueno → Water7)	| 101 | /25	| 
| A3 (Cipher → Water7)	| 701 | /22 |
| A4 (Water7 → Foosha)	| 2 | /30	|
| A5 (Guanhao → Foosha)	|	2 | /30 |
| A6 (Elena → Guanhao)	|	301 | /23 |
| A7 (Fukurou → Guanhao)	|	201 | /24 |
| A8 (Maingate + Jorge → Guanhao)	|	3 | /29 | 
|**Total** 		|**1314**|**/21**|

Berdasarkan total jumlah IP yang telah didapat yakni sebesar **1314**. Jumlah IP tersebut, apabila dikonversikan ke dalam netmask didapatkan **netmask length /21** sesuai dengan tabel modul berikut :

![image](https://user-images.githubusercontent.com/55240758/143677389-021ad918-2bdd-4c6a-984c-43f166d257f1.png)

Dimana, dengan  **netmask length /21** diperoleh beberapa informasi penting yakni wildcard yang akan digunakan pada proses pembagian subnetting pada pohon subnet. 

**Langkah 3 :** Hitung pembagian IP berdasarkan jumlah IP yang telah didapat pada langkah sebelumnya. Adapun untuk mempermudah perhitungan dengan menggunakan visualisasi atau pohon subnetting.

Jumlah IP yang telah kami dapat memiliki **netmask length /21** , dimana netmask length tersebut memiliki **wildcard 0.0.7.255** berdasarkan tabel modul yang telah dilampirkan sebelumnya. Adapun berdasarkan wildcard tersebut kita dapat menyimpulkan berapa range IP pada digit tertentu yang disediakan untuk suatu netmask. Pada kasus ini digit ketiga wilcard yaitu **7**, atau dapat kita artikan range digit ketiga suatu IP pada netmask tersebut adalah 0-7, yang menandakan ada 8 macam kombinasi IP pada digit tersebut.

Sebelum melakukan pehitungan, perlu diketahui bahwa prefix IP yang kami gunakan adalah **10.2**. Kemudian berdasarkan prefix IP & netmask length yang telah didapat, IP permulaan yang akan dijadikan root pada pohon subnetting yaitu **10.2.0.0 /21**. 

Dari IP **10.2.0.0 /21** ini, kita membagi rangenya menjadi 2 sama rata. Sebelum itu dapat kita ketahui bahwa cabang dari root yang memiliki **netmask length /21** maka netmask length selanjutnya akan memiliki selisih satu dengan netmask length yang dimiliki root pada cabang level 1, yakni **netmask length /22**. Dari **netmask length /22** diperoleh bahwa netmask length tersebut memiliki **wildcard 0.0.3.255** atau dapat kita artikan range digit ketiga suatu IP pada netmask tersebut adalah 0-3 (berjumlah 4). Karena dari IP **10.2.0.0 /21** ini, kita membagi rangenya menjadi 2 sama rata, dimana memiliki **wildcard 0.0.7.255** atau dapat kita artikan range digit ketiga suatu IP pada netmask tersebut adalah 0-7 (berjumlah 8). Maka cabang di sisi kiri akan memiliki range IP **10.2.0.0 /22** hingga **10.2.3.0 /22** (berjumlah 4). Sedangkan di sisi kanan, rangenya adalah **10.2.4.0 /22** hingga **10.2.7.0 /22** (berjumlah 4). Karena pada hasil subnetting ada yang membutuhkan netmask /22, sedangkan di pemecahan ini kita memiliki 2 cabang tersebut, maka dalam hal ini kita memilih IP **10.2.0.0 /22** sebagai IP subnet A3 sedangkan untuk cabang selanjutnya akan digunakan untuk meneruskan pembagian hingga didapatkan pohon subnetting sebagai berikut :

![VSLM SHIFT 5 drawio](https://user-images.githubusercontent.com/55240758/145655091-05b7e6ba-95af-4f9c-aac2-33c6e763a276.png)

Maka berdasarkan perhitungan atau pembagian IP yang diperoleh dari pohon subnetting, didapatkan :

| Subnet | Jumlah IP | Length | IP | Subnet Mask |   
| --- | --- | --- | --- | --- |    
| A1 (Doriki + Jipangu → Water7) | 3 | /29 | 10.2.7.128 | 255.255.255.248 |  
| A2 (Blueno → Water7) | 101 | /25 | 10.2.7.0 | 255.255.255.128 |  
| A3 (Cipher → Water7) | 701 | /22 | 10.2.0.0 | 255.255.252.0 |  
| A4 (Water7 → Foosha) | 2 | /30 | 10.2.7.144 | 255.255.255.252 |  
| A5 (Guanhao → Foosha) | 2 | /30 | 10.2.7.148 | 255.255.255.252 |  
| A6 (Elena → Guanhao) | 301 | /23 | 10.2.4.0 | 255.255.254.0 |  
| A7 (Fukurou → Guanhao) | 201 | /24 | 10.2.6.0 | 255.255.255.0 |  
| A8 (Maingate + Jorge → Guanhao) | 3 | /29 | 10.2.7.136 | 255.255.255.248 |  
| **Total** | **1314** | **/21** |  |  |  


**Langkah 4 :** Melakukan penyetingan IP dan netmask berdasarkan perhitungan pada router maupun PC sesuai dengan label yang telah diberikan sebelumnya pada interface yang sesuai pada ``/etc/network/interfaces`` dengan menggunakan ``nano`` atau klik kanan node > edit network configuration untuk menambahkannya.

- Doriki
```
auto eth0
iface eth0 inet static
address 10.2.7.130
netmask 255.255.255.248
gateway 10.2.7.129
```  

- Jipangu
```
auto eth0
iface eth0 inet static
address 10.2.7.131
netmask 255.255.255.248
gateway 10.2.7.129
```

- Water7
```
auto lo
iface lo inet loopback

# Ke Foosha
auto eth0
iface eth0 inet static
address 10.2.7.145
netmask 255.255.255.252
gateway 10.2.7.146

# Ke A1(Doriki & Jipangu)
auto eth1
iface eth1 inet static
address 10.2.7.129
netmask 255.255.255.248

# Ke Blueno
auto eth2
iface eth2 inet static
address 10.2.7.1
netmask 255.255.255.128

# Ke Cipher
auto eth3
iface eth3 inet static
address 10.2.0.1
netmask 255.255.252.0
```

- Blueno
```
auto eth0
iface eth0 inet static
address 10.2.7.2
netmask 255.255.255.128
gateway 10.2.7.1
```

- Cipher
```
auto eth0
iface eth0 inet static
address 10.2.0.2
netmask 255.255.252.0
gateway 10.2.0.1
```

- Foosha
```
auto lo
iface lo inet loopback

# Ke NAT
auto eth0
iface eth0 inet dhcp
hwaddress ether de:7c:14:3b:5e:96


# Ke Water7
auto eth1
iface eth1 inet static
address 10.2.7.146
netmask 255.255.255.252

# Ke Guanhao
auto eth2
iface eth2 inet static
address 10.2.7.149
netmask 255.255.255.252
```

- Jorge
```
auto eth0
iface eth0 inet static
address 10.2.7.138
netmask 255.255.255.248
gateway 10.2.7.137
```
- Maingate
```
auto eth0
iface eth0 inet static
address 10.2.7.139
netmask 255.255.255.248
gateway 10.2.7.137
```

- Guanhao
```
auto lo
iface lo inet loopback

# Ke Foosha
auto eth0
iface eth0 inet static
address 10.2.7.150
netmask 255.255.255.252
gateway 10.2.7.149

# Ke A8( Jorge & Maingate)
auto eth1
iface eth1 inet static
address 10.2.7.137
netmask 255.255.255.248

# Ke Elena
auto eth2
iface eth2 inet static
address 10.2.4.1
netmask 255.255.254.0

# Ke Fukurou
auto eth3
iface eth3 inet static
address 10.2.6.1
netmask 255.255.255.0
```

- Elena
```
auto eth0
iface eth0 inet static
address 10.2.4.2
netmask 255.255.254.0
gateway 10.2.4.1
```

- Fukurou
```
auto eth0
iface eth0 inet static
address 10.2.6.2
netmask 255.255.255.0
gateway 10.2.6.1
```

**Langkah 5 :** Melakukan routing pada setiap router (C), Adapun pada penyettingan pada GNS3, telah kami rangkum sebagai berikut.
Routing Foosha
- kiri
```
route add -net 10.2.7.128 netmask 255.255.255.248 gw 10.2.7.145
route add -net 10.2.7.0 netmask 255.255.255.128 gw 10.2.7.145
route add -net 10.2.0.0 netmask 255.255.252.0 gw 10.2.7.145
```

- kanan
```
route add -net 10.2.4.0 netmask 255.255.254.0 gw 10.2.7.150
route add -net 10.2.6.0 netmask 255.255.255.0 gw 10.2.7.150
route add -net 10.2.7.136  netmask 255.255.255.248 gw 10.2.7.150
```

**Langkah 6:** Memberikan ip pada subnet Blueno, Cipher, Fukurou, dan Elena secara dinamis menggunakan bantuan DHCP server.  

Agar dapat mengakses internet, pada command line interface router (CLI) **Foosha**  ketikkan perintah ``iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 10.2.0.0/21``. Dan pada setiap client/PC maupun router lainnya ketikkan ``echo nameserver 192.168.122.1 > /etc/resolv.conf``.

Setting DHCP server (Jipangu) agar setiap subnet mendapat IP dinamis :
- Melakukan instalasi DHCP Server : Melakukan update terlebih dahulu dengan ```apt-get update ```, kemudian ```apt-get install isc-dhcp-server -y```

- Tambahkan config pada ``/etc/network/interfaces`` dengan menggunakan ``nano`` atau klik kanan node > edit network berikut untuk setiap subnet (Blueno, Cipher, Fukurou, dan Elena)
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```  

- Pada ``/etc/default/isc-dhcp-server`` di DHCP server (Jipangu) tambahkan ``INTERFACES=" "`` dengan ``eth0``. Sehingga menjadi ``INTERFACES="eth0"``.  

- Pada ``/etc/dhcp/dhcpd.conf`` di DHCP server (Jipangu) tambahkan :  
```
# Blueno A2
subnet 10.2.7.0 netmask 255.255.255.128 {
        range 10.2.7.2 10.2.7.126;
        option routers 10.2.7.1;
        option broadcast-address 10.2.7.127;
        option domain-name-servers 10.2.7.130;
        default-lease-time 600;
        max-lease-time 7200;
};

# Cipher A3
subnet 10.2.0.0 netmask 255.255.252.0 {
        range 10.2.0.2 10.2.3.254;
        option routers 10.2.0.1;
        option broadcast-address 10.2.3.255;
        option domain-name-servers 10.2.7.130;
        default-lease-time 600;
	max-lease-time 7200;
}; 

# Elena A6
subnet 10.2.4.0 netmask 255.255.254.0 {
        range 10.2.4.2 10.2.5.254;
        option routers 10.2.4.1;
        option broadcast-address 10.2.5.255;
        option domain-name-servers 10.2.7.130;
        default-lease-time 600;
        max-lease-time 7200;
};

# Fukurou A7
subnet 10.2.6.0 netmask 255.255.255.0 {
        range 10.2.6.2 10.2.6.254;
        option routers 10.2.6.1;
        option broadcast-address 10.2.6.255;
        option domain-name-servers 10.2.7.130;
        default-lease-time 600;
        max-lease-time 7200;
};

# Routing dari Jipangu ke router
subnet 10.2.7.128 netmask 255.255.255.248 {
        option routers 10.2.7.129;
};
```

**Langkah 7:** Melakukan setting DHCP Relay pada router yang menghubungkannya (Water7 & Guanhao).
- Melakukan instalasi DHCP Relay : Melakukan update terlebih dahulu dengan ```apt-get update ```, kemudian ```apt-get install isc-dhcp-relay -y```.

- Pada ``/etc/default/isc-dhcp-relay`` di DHCP server (Jipangu) tambahkan :
```
# What servers should the DHCP relay forward requests to?
SERVERS="10.2.7.131"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth0 eth1 eth2 eth3"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
```
  
**Langkah 8:** Melakukan pengecekan apakah pembagian ip pada subnet Blueno, Cipher, Fukurou, dan Elena (DHCP Client) secara dinamis telah berhasil dengan membuka CLI tiap DHCP client tersebut.
- Blueno
![hasil dhcp blueno](https://user-images.githubusercontent.com/55240758/145657662-0bed2d95-538d-4d16-a831-cfd8c961baee.jpg)


- Cipher
![hasil dhcp cipher](https://user-images.githubusercontent.com/55240758/145657664-72cebb17-0847-4010-ac0d-ea52df43edcd.jpg)


- Fukurou
![hasil dhcp fukurou](https://user-images.githubusercontent.com/55240758/145657667-699e77c6-45a4-4aff-9cae-bc04212399c2.jpg)


- Elena
![hasil dhcp elena](https://user-images.githubusercontent.com/55240758/145657668-d1a4bfde-2157-452f-8a6c-0cffd9175fb7.jpg)

## Soal 1 (belum lengkap)
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE. Maka pada CLI Foosha masukkan perintah berikut:
```
iptables -t nat -A POSTROUTING -s 10.2.0.0/21 -o eth0 -j SNAT --to-source 192.168.122.96
```
dimana _to-source_ tersebut didapatkan dari .....

Selain itu, agar klien yang terdiri dari Blueno, Cipher, Elena, Fukurou dapat mengakses ke luar, maka perlu ditambahkan forwarding pada DNS Server yaitu Doriki. Maka terlebih dahulu...

- Melakukan instalasi DNS Server : Melakukan update terlebih dahulu dengan ```apt-get update ```, kemudian ```apt-get install bind9 -y``` 

- Pada file ``/etc/bind/named.conf.options``, uncomment pada bagian (0.0.0.0 diubah menjadi 192.168.122.1):

```
forwarders {
    192.168.122.1
};
```  

Kemudian command pada bagian ini, hingga menjadi seperti ini
```
// dnssec-validation auto;
```

Dan tambahkan
```
allow-query{any;};
```

Maka secara keseluruhan pada file ``named.conf.options`` sebagai berikut
```
options {
        directory "/var/cache/bind";

        // If there is a firewall between you and nameservers you want
        // to talk to, you may need to fix the firewall to allow multiple
        // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

        // If your ISP provided one or more IP addresses for stable
        // nameservers, you probably want to use them as forwarders.
        // Uncomment the following block, and insert the addresses replacing
        // the all-0's placeholder.

        forwarders {
                192.168.122.1;
        };

        //========================================================================
        // If BIND logs error messages about the root key being expired,
        // you will need to update your keys.  See https://www.isc.org/bind-keys
        //========================================================================
        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
				listen-on-v6 { any; };
};
```

## Soal 2
Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan. 

**Langkah 1 :** Masukkan perintah berikut pada CLI DHCP server & DNS server
```
# Port 80 -> HTTP
iptables -A FORWARD -d 10.2.7.128/29 -i eth0 -p tcp --dport 80 -j DROP
# Port 443 -> HTTPS
iptables -A FORWARD -d 10.2.7.128/29 -i eth0 -p tcp --dport 443 -j ACCEPT
```

**Langkah 2:** Lakukan pengecekan apakah perintah yang telah diinputkan berhasil dengan 
- Melakukan ``ping its.ac.id`` untuk HTTPS 

![no  2 ping pada jipangu](https://user-images.githubusercontent.com/55240758/145658823-24947d44-0d49-4238-ac9d-1c1da1dd0afc.jpg)

- Melakukan ``ping monta.if.its.ac.id`` untuk HTTP

![no  2 ping pada doriki](https://user-images.githubusercontent.com/55240758/145658820-4ddbd6ac-12d8-480a-9d6d-3af03b62310c.jpg)

## Soal 3 (belum selesai)
Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.   

**Langkah 1:** Masukkan perintah berikut pada CLI DHCP server & DNS server
```
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

**Langkah 2:** Lakukan pengecekan apakah perintah yang telah diinputkan berhasil dengan melakukan ping 4 kali pada CLI yang berbeda-beda, maka pada ping yang ke 4 tidak akan berjalan jika perintah sebelumnya telah berhasil.

Gambar ...


## Soal 4 (belum selesai)
Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.   

**Langkah 1:** Pada CLI DNS server (Doriki) tambahkan perintah berikut
```
# Blueno
iptables -A INPUT -s 10.2.7.0/25 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 10.2.7.0/25 -j REJECT

# Cipher
iptables -A INPUT -s 10.2.0.0/22 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 10.2.0.0/22 -j REJECT
```

**Langkah 2:**  Lakukan pengecekan apakah perintah yang telah diinputkan berhasil dengan melakukan ping pada Blueno & Cipher dengan studi kasus :  

**Sabtu, 11 Desember 2021**
1.  Pukul 08:00 -> gabisa
```
date -s "6 DEC 2021 08:00:00"
```
Gambar ...

2.  Pukul 16:00 -> gabisa
```
date -s "6 DEC 2021 16:00:00"
```
Gambar ...


**Senin, 15 November 2021**
1.  Pukul 10:00 -> bisa
```
date -s "15 NOV 2021 10:00:00"
```
Gambar ...

2.  Pukul 18:00 -> gabisa
```
date -s "15 NOV 2021 18:00:00"
```
Gambar ...

## Soal 5 (belum selesai)
Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.  

**Langkah 1:** Pada CLI DNS server (Doriki) tambahkan perintah berikut
```
# Elena
iptables -A INPUT -s 10.2.4.0/23 -m time --timestart 15:01 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 10.2.4.0/23 -j REJECT

# Fukurou
iptables -A INPUT -s 10.2.6.0/24 -m time --timestart 15:01 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 10.2.6.0/24 -j REJECT
```

**Langkah 2:**  Lakukan pengecekan apakah perintah yang telah diinputkan berhasil dengan melakukan ping pada Blueno & Cipher dengan studi kasus :  

**Sabtu, 11 Desember 2021**
1.  Pukul 08:00 -> gabisa
```
date -s "6 DEC 2021 08:00:00"
```
Gambar ...

2.  Pukul 16:00 -> bisa
```
date -s "6 DEC 2021 16:00:00"
```
Gambar ...

## Soal 6
Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate.
