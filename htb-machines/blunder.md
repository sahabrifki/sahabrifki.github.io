---
description: '{"Level" : "Easy" , "OS" : "Linux" , "IP_Address" : "10.10.10.191"}'
---

# Blunder

## Deskripsi Mesin

Mesin blunder ini merupakan mesin dengan sistem operasi linux dan tingkat kesulitan easy pada hackthebox. mesin dirilis oleh [egotisticalSW](https://www.hackthebox.eu/home/users/profile/94858).

## Scanning Nmap

```text
nmap -sV 10.10.10.191
```

![Scanning Nmap](../.gitbook/assets/image%20%2832%29.png)

## Enumerasi port 80 \(Web Aplikasi\)

![http://10.10.10.191/](../.gitbook/assets/image%20%2830%29.png)

Dari tampilan web aplikasi tersebut sebenarnya tidak ada pemrosesan data apapun. tapi karena web aplikasi tersebut menampilkan beberapa nama character. kita catat saja dulu nama nama yang tercantum pada tampilan web tersebut. lalu ? karena web aplikasi nya hanya statis dan tidak memproses data apapun. selanjutnya saya mencoba melakukan enumerasi direktori / file yang mungkin memiliki penjelasan lebih detail terhadap web aplikasi tersebut.

Seperti biasa, saya melakukan direktori bruteforcing menggunakan ffuf.

```text
ffuf -u http://10.10.10.191/FUZZ -w /path/to/wordlist
```

![Direktori Bruteforce](../.gitbook/assets/image%20%2831%29.png)

Dari hasil bruteforce kita mendapatkan file `todo.txt`

```bash
#isi dari file todo.txt
-Update the CMS
-Turn off FTP - DONE
-Remove old users - DONE
-Inform fergus that the new blog needs images - PENDING
```

Dari file todo.txt tersebut kita mendapati beberapa pesan tentang web aplikasi ini dan salah satu user dengan sebutan "fergus". siapa fergus !? yaudah kita simpan dulu saja informasinya. kemudian saya mencoba melakukan bruteforcing direktori lagi dengan wordlist yang berbeda.

![directory bruteforce](../.gitbook/assets/image%20%2828%29.png)

kita mendapati adanya direktory admin dari hasil diatas. kemudian langsung saja saya mencoba mengakses directory tersebut dan ternya langsung disuguhkan pada sebuah halaman login dengan banner bertuliskan bludit.

![](../.gitbook/assets/image%20%2829%29.png)

Times to Googling & saya mendapati bawah bludit adalah sebuah CMS open source. Dan..... Setelah itu saya mendapatkan informasi serta script untuk eksploitasi \( Authentication Bypass \) dari si cms bludit [disini](https://medium.com/@musyokaian/bludit-cms-version-3-9-2-brute-force-protection-bypass-283f39a84bbb)



