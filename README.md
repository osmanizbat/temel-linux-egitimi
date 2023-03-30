# Uygulamalı Temel Linux Eğitimi

## SSH bağlantısı

Sunucunuza IP yerine hostname ile bağlanabilmek ve bu dökümandaki
komutları direk kopyalayarak çalıştırabilmek için
**C:\Windows\System32\drivers\etc\host** dosyanıza sunucunuzun IP
adresini içeren aşağıdaki gibi satır eklemenizi öneririm:
**192.168.185.201 kfnlinux**

Windows’ta opsiyonel olarak gelen OpenSSH Client’ı ya da Putty,
mRemoteNG, MobaXterm vb. uygulamaları indirip kurarak bağlanabilirsiniz.

``` powershell
PS C:\> ssh kafein@kfnlinux
kafein@kfnlinux's password:
Welcome to Ubuntu 22.04.2 LTS (GNU/Linux 5.15.0-67-generic x86_64)

kafein@kfnlinux:~$
```

## Shell Arabirimi

Terminal uygulaması ile sisteme giriş yaptığımızda bizi karşılayan metin
tabanlı kabuk arabirimidir. Bash, Zsh, Fish, C Shell gibi çeşitleri
vardır. Günümüzde en yaygın olarak Bash kullanılmakta ve çoğu Linux
dağıtımında var sayılan shell olarak gelmektedir.

``` bash
kafein@kfnlinux:~$ echo "Hello World!"
Hello World!
kafein@kfnlinux:~$
```

## Standard Input & Output

input: 0 output: 1 error: 2

### Çıktı Yönlendirme

``` bash
kafein@kfnlinux:~$ mkdir work
kafein@kfnlinux:~$ cd work
kafein@kfnlinux:~$ echo "Hello World!" > hello
kafein@kfnlinux:~$ echo "foo" >> hello
kafein@kfnlinux:~$ echo "bar" >> hello
kafein@kfnlinux:~$ echo "baz" >> hello
```

``` bash
kafein@kfnlinux:~$ ls -al > file-list
```

### Pipe kullanarak komutları birbirine bağlama

``` bash
kafein@kfnlinux:~$ ls -al | wc -l
15
```

## Sık Kullanılan Komutlar

- ls
- cd
- pwd
- mkdir
- cp
- mv
- rm
- find
- cat
- touch
- grep
- awk
- echo
- head
- tail
- more
- less
- wc
- tar
- vim
- nano
- ps
- top
- kill
- nohup
- watch
- df
- du
- chown
- chmod
- ip
- ifconfig
- ping
- nslookup
- netstat
- telnet
- nc
- curl
- wget
- rsync
- which

## Shell Scripting

İnteraktif shell arabiriminde çalıştırdığımız komutlardan daha karmaşık,
birden fazla komut adımından oluşan, sık kullandığımız işlemler için
shell scriptler yazabiliriz.

``` bash
kafein@kfnlinux:~$ nano find_recent_files.sh
```

Aşağıdaki scripti find_recent_files.sh dosyasına kaydedelim

``` bash
#!/usr/bin/bash

cd $HOME

echo "Finding recent files..."

mkdir recent_files

find work -type f -mtime -1 -print0 | xargs -0 -r cp -t recent_files/

date=$(date '+%Y%m%d%H%M%S')

tar zcvf recent_files_$date.tar.gz recent_files

rm -rf recent_files
```

Scripti çalıştıralım

``` bash
kafein@kfnlinux:~$ chmod 700 find_recent_files.sh
kafein@kfnlinux:~$ ./find_recent_files.sh
```

## APT Paket Yönetim Sistemi

Debian ve türevi Ubuntu vb. Linux dağıtımlarında kullanılan paket
yönetim sistemidir. İlk kurulumla birlikte gelmeyen 10 binlerce yazılımı
internet üzerinde APT paket deplolarından yükleyebilirsiniz.

``` bash
kafein@kfnlinux:~$ netstat
Command 'netstat' not found, but can be installed with:
sudo apt install net-tools

kafein@kfnlinux:~$ sudo apt install net-tools
```

## Kullanıcı ve Gruplar

**Kulanıcı ekleme:**

``` bash
kafein@kfnlinux:~$ sudo useradd test-user
kafein@kfnlinux:~$ sudo passwd test-user
kafein@kfnlinux:~$ exit

PS C:\> ssh test-kafein@kfnlinux
```

**Grup ekleme:**

``` bash
kafein@kfnlinux:~$ sudo groupadd test-group
kafein@kfnlinux:~$ sudo usermod -aG test-group test-user
```

**Dizin yetkilendirme:**

``` bash
kafein@kfnlinux:~$ sudo mkdir /opt/testwork
kafein@kfnlinux:~$ sudo usermod -aG test-group test-user
kafein@kfnlinux:~$ sudo chown test-user:test-group /opt/testwork
kafein@kfnlinux:~$ sudo chmod 770 /opt/testwork
kafein@kfnlinux:~$ ls -al /opt/testwork
kafein@kfnlinux:~$ sudo usermod -aG test-group kafein
kafein@kfnlinux:~$ exit

PS C:\> ssh kafein@kfnlinux
kafein@kfnlinux:~$ ls -al /opt/testwork
```

## Cron ile zamanlanmış görevler çalıştırma

``` bash
kafein@kfnlinux:~$ crontab -e

32 * * * * /home/kafein/find_recent_files.sh
```

## Curl

``` bash
kafein@kfnlinux:~$ sudo apt install jq
```

``` bash
curl -s "https://api.open-meteo.com/v1/forecast?latitude=41.01&longitude=28.95&hourly=temperature_2m&forecast_days=1" | jq ".hourly.temperature_2m" | python -c "import sys, statistics, json; print(statistics.mean(json.loads(sys.stdin.read())))"
```

https://open-meteo.com/en/docs#latitude=41.01&longitude=28.95&hourly=temperature_2m&forecast_days=1

mockbin.org

## Python ile basit bir API uygulaması

``` bash
kafein@kfnlinux:~$ sudo apt install python3-pip
kafein@kfnlinux:~$ pip install flask
kafein@kfnlinux:~$ mkdir hello-api
kafein@kfnlinux:~$ cd hello-api
kafein@kfnlinux:~/hello-api$
kafein@kfnlinux:~$ nano hello.py
```

Aşağıdaki kodu hello.py dosyasına kaydedelim

``` python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"
```

``` bash
kafein@kfnlinux:~$ sudo apt install python3-pip
kafein@kfnlinux:~$ mkdir hello-api
kafein@kfnlinux:~$ cd hello-api
kafein@kfnlinux:~/hello-api$
kafein@kfnlinux:~$ nano hello.py
```

## Nginx’i reverse proxy olarak kuralım

``` bash
kafein@kfnlinux:~$ sudo apt install nginx
kafein@kfnlinux:~$ ps -ef | grep nginx
kafein@kfnlinux:~$ curl localhost
```

Aşağıdaki location bloğunu /etc/nginx/sites-available/default dosyasında

    server {
      ...
      location /hello {
              proxy_pass http://localhost:5000/;
      }
      ...
    }  

``` bash
kafein@kfnlinux:~$ sudo nano /etc/nginx/sites-available/default
kafein@kfnlinux:~$ sudo systemctl restart nginx
kafein@kfnlinux:~$ curl localhost/hello
```
