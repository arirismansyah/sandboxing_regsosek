<div align = "center">

# **SANDBOXING SCRIPT QUERY TOOLS REGSOSEK 2022 BPS PROVINSI SUMATERA SELATAN**

---
<div align = "center">

<img src = "assets/profile.png" width="100" height="100"></img>
### **Ari Rismansyah | @arirismansyah**



© Copyright 2022 - Ari Rismansyah – ari.rismansyah@bps.go.id

---

</div>

</div>


<br>

## **Deskripsi**

<div align = "justify">

Berikut ini merupakan scipt sandbox untuk explorasi dan identifikasi data hasil entri REGSOSEK 2022 BPS Provinsi Sumatera Selatan


</div>

<br>

### 00 - REKAP ERROR, WARNING, & BLANK PER PETUGAS ENTRI PER WILAYAH
```
SELECT t_rt.kode_kab, kode_kec, kode_desa, kode_sls,  sls_nama, m_operator.realname as nama_operator, 
SUM (CASE WHEN status_dok = 'E' THEN 1 ELSE 0 END) AS jumlah_error,
SUM (CASE WHEN status_dok = 'W' THEN 1 ELSE 0 END) AS jumlah_warning,
SUM (CASE WHEN status_dok = '' THEN 1 ELSE 0 END) AS jumlah_blank
FROM t_rt, m_operator WHERE m_operator.id_operator = t_rt.kode_operator 
Group by t_rt.kode_kab, kode_kec, kode_desa, kode_sls, sls_nama, m_operator.realname
order by realname

```
### 01 - KELUARGA MISKIN ATAU SANGAT MISKIN TAPI MEMILIKI ASET MOBIL, KOMPUTER/LAPTOP KAPAL, PERAHU MOTOR, ATAU EMAS PERHIASAN
```
SELECT DISTINCT kode_kab,kode_kec,kode_desa,kode_sls,sls_nama,alamat,r108,nama_kk,r109 as nu_bangunan,r110 as nu_verifikasi,r111 as status_keluarga,r502k as kepemilikan_mobil,r502g as kepemilikan_perhiasan,r502h as kepemilikan_laptop,r502m as kepemilikan_perahu_motor
FROM tab_view_k WHERE (r111 = 1 OR r111 = 2) AND (r502g = 1 OR r502h = 1 OR r502k = 1 OR r502m = 1) AND (status_dok = 'W' OR status_dok = 'C')

```

### 02 - KELUARGA MISKIN ATAU SANGAT MISKIN TAPI IJAZAH KEPALA KELUARGA > S1
```
SELECT DISTINCT kode_kab,kode_kec,kode_desa,kode_sls,sls_nama,alamat,r108,nama_kk,r109 as nu_bangunan,r110 as nu_verifikasi,r111 as status_keluarga,r402 as nama_kepala_keluarga,r409, r415 as ijazah_kepala_keluarga
FROM tab_view_k WHERE (r111 = 1 OR r111 = 2) AND (r409 = 1) AND (r415 >= 19 AND r415!=23) AND (status_dok = 'W' OR status_dok = 'C')

```
### 03 - KELUARGA MISKIN ATAU SANGAT MISKIN TAPI LANTAI GRANIT
```
SELECT DISTINCT kode_kab,kode_kec,kode_desa,kode_sls,sls_nama,alamat,r108,nama_kk,r109 as nu_bangunan,r110 as nu_verifikasi,r111 as status_keluarga,r301a,r303 as lantai
FROM tab_view_k WHERE (r111 = 1 OR r111 = 2) AND (r301a=1) AND (r303 = 1) AND (status_dok = 'W' OR status_dok = 'C')

```
### 04 - KELUARGA MISKIN ATAU SANGAT MISKIN TAPI PEKERJAAN ART PNS
```
SELECT kode_kab,kode_kec,kode_desa,kode_sls,sls_nama,alamat,r108,nama_kk,r109 as nu_bangunan,r110 as nu_verifikasi,r111 as status_keluarga,r402 as nama_kepala_keluarga,r409,r417,r418
FROM tab_view_k WHERE (r111 = 1 OR r111 = 2) AND (r418=5) AND (status_dok = 'W' OR status_dok = 'C')

```
### 05 - KELUARGA MISKIN ATAU SANGAT MISKIN TAPI LUAS BANGUNAN TEMPAT TINGGAL > 100 M2
```
SELECT DISTINCT kode_kab,kode_kec,kode_desa,kode_sls,sls_nama,alamat,r108,nama_kk,r109 as nu_bangunan,r110 as nu_verifikasi,r111 as status_keluarga,r301a,r302 as luas_bangunan_tempat_tinggal
FROM tab_view_k WHERE (r111 = 1 OR r111 = 2) AND (r301a = 1) AND (r302 > 100) AND (status_dok = 'W' OR status_dok = 'C')

```
### 06 - KELUARGA MISKIN DENGAN DAYA LISTRIK PADA RUMAH 450 W TAPI MEMILIKI AC
```
SELECT DISTINCT kode_kab,kode_kec,kode_desa,kode_sls,sls_nama,alamat,r108,nama_kk,r109 as nu_bangunan,r110 as nu_verifikasi,r111 as status_keluarga,r307b1,r307b2,r307b3,r502c as kepemilikan_ac
FROM tab_view_k WHERE (r111 = 1 OR r111 = 2) AND (r307b1 = 1 AND (r307b2 IS NULL AND r307b3 IS NULL)) AND (r502c = 1) AND (status_dok = 'W' OR status_dok = 'C')

```
### 07 - ART WANITA BEUM MENIKAH TAPI SEDANG HAMIL
```
SELECT kode_kab,kode_kec,kode_desa,kode_sls,sls_nama,alamat,r108,nama_kk,r109 as nu_bangunan,r110 as nu_verifikasi,r401 as no_art,r402 as nama_art,r405 as jenis_kelamin,r408 as status_perkawinan,r410 as sedang_hamil
FROM tab_view_k WHERE (r405 = 2 AND r408 = 1) AND (r410 =1) AND (status_dok = 'W' OR status_dok = 'C')

```
### 08 - ART BERJENIS KELAMIN SELAIN PRIA ATAU WANITA
```
SELECT kode_kab,kode_kec,kode_desa,kode_sls,sls_nama,alamat,r108,nama_kk,r109 as nu_bangunan,r110 as nu_verifikasi,r401 as no_art,r402 as nama_art,r405 as jenis_kelamin
FROM tab_view_k WHERE (r405 != 2 AND r405 != 1) AND (status_dok = 'W' OR status_dok = 'C')

```

### 09 - STATUS ART MERUPAKAN PASANGAN KEPALA KELUARGA (r409=2) TAPI BELUM MENIKAH (r408=1)
```
SELECT kode_kab,kode_kec,kode_desa,kode_sls,sls_nama,alamat,r108,nama_kk,r109 as nu_bangunan,r110 as nu_verifikasi,r401 as no_art,r402 as nama_art,r408 as status_perkawinan, r409 as status_hubungan_dgn_kk
FROM tab_view_k WHERE (r408 = 1 AND r409= 2) AND (status_dok = 'W' OR status_dok = 'C')

```

### 10 - KELUARGA MISKIN ATAU SANGAT MISKIN, RUMAH MILIK SENDIRI, LISTRIK LEBIH DARI 1300 WATT
```
SELECT DISTINCT kode_kab,kode_kec,kode_desa,kode_sls,sls_nama,alamat,r108,nama_kk,r109 as nu_bangunan,r110 as nu_verifikasi,r111 as status_keluarga, r301a as kepemilikan_rumah, r307b1 as listrik, r307b2 as listrik2, r307b3 as listrik3
FROM tab_view_k WHERE (r111 = 1 OR r111 = 2) AND (r307b1 = 4 or r307b1=5 or r307b2 = 4 or r307b2=5  or r307b3 = 4 or r307b3=5) AND (r301a=1) AND (status_dok = 'W' OR status_dok = 'C')

```
### 11 - KELUARGA MISKIN ATAU SANGAT MISKIN SUMBER AIR MINUM UTAMA AIR KEMASAN BERMERK, BAHAN BAKAR MEMASAK LISTRIK, TIDAK MEMASAK
```
SELECT DISTINCT kode_kab,kode_kec,kode_desa,kode_sls,sls_nama,alamat,r108,nama_kk,r109 as nu_bangunan,r110 as nu_verifikasi,r111 as status_keluarga,r306a as air_minum_utama, r308 as bahan_bakar
FROM tab_view_k WHERE (r111 = 1 OR r111 = 2) AND (r306a= 1 OR 308=1 or 308=0) AND (status_dok = 'W' OR status_dok = 'C')

```

### 12 - KELUARGA TIDAK MISKIN LUAS LANTAI BANGUNAN < 36M2, LANTAI TERLUAS BAMBU, TANAH, LAINNYA, DINDING TERLUAS ANYAMAN KAYU, BATANG KAYU, BAMBU, ATAP TERLUAS BAMBU, KAYU SIRAP, JERAMI, LAINNYA
```
SELECT DISTINCT kode_kab,kode_kec,kode_desa,kode_sls,sls_nama,alamat,r108,nama_kk,r109 as nu_bangunan,r110 as nu_verifikasi,r111 as status_keluarga,r302 as luas_lantai, r303 as jenis_lantai, r304 as jenis_dinding, r305 as jenis_atap
FROM tab_view_k WHERE (r111 =3) AND (r302<36 OR (R303>6 AND r303<10) OR (R304>3 AND R304<8 ) OR (r305>4 and r305<9)) AND (status_dok = 'W' OR status_dok = 'C')
```

### 13 - KELUARGA TIDAK MISKIN SUMBER AIR MINUM UTAMA MATA AIR TAK TERLINDUNG, AIR PERMUKAAN, AIR HUJAN, LISTRIK 450 WATT, BAHAN BAKAR MEMASAK MINYAK TANAH, BRIKET, ARANG, KAYU BAKAR
```
SELECT DISTINCT kode_kab,kode_kec,kode_desa,kode_sls,sls_nama,alamat,r108,nama_kk,r109 as nu_bangunan,r110 as nu_verifikasi,r111 as status_keluarga,r306a as air_minum, r307b1 as daya_listrik, r308 as bahan_bakar_memasak
FROM tab_view_k WHERE (r111 =3) AND ((r306a>7 and r306a<12) OR (r307b1='1' AND r307b2='' AND r307b3='') OR(r308>'6' AND r308<'11')) AND (status_dok = 'W' OR status_dok = 'C')
```

### 14 - KELUARGA TIDAK MISKIN TAPI KLOSET KODE 4-6
```
SELECT DISTINCT kode_kab,kode_kec,kode_desa,kode_sls,sls_nama,alamat,r108,nama_kk,r109 as nu_bangunan,r110 as nu_verifikasi,r111 as status_keluarga,r309a as fasilitas_tempat_bab
FROM tab_view_k WHERE (r111 =3) AND (r309a=4 OR r309a=5 OR r309a=6 ) AND (status_dok = 'W' OR status_dok = 'C') 
```

### 15 - KELUARGA TIDAK MISKIN TAPI PUNYA PBI JKN
```
SELECT DISTINCT kode_kab,kode_kec,kode_desa,kode_sls,sls_nama,alamat,r108,nama_kk,r109 as nu_bangunan,r110 as nu_verifikasi,r111 as status_keluarga,r431a as jaminan_kesehatan
FROM tab_view_k WHERE (r111 =3) AND (r431a ='01' OR r431a ='1' ) AND (status_dok = 'W' OR status_dok = 'C')
```

