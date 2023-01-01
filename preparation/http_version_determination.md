# თეორია

ვებ-სერვერის სახელის და ვერსიის დაგენა მარტივია. ამისთვის telnet-ით უნდა დაუკავშირდეთ ვერ-სერვერს (უფრო ხშირად პორტი 80-ია ნახმარი, SSL-იან საიტებზე კი პორტი 443), telnet-ით კავშირი რომ დამყარდება უნდა გაუგზავნოთ HTTP ბრძანება HEAD ან OPTIONS.
ამ მეთოდით მოპოვებული ინფორმაცია 100%-ით სანდო არაა იმიტომ რომ ადმინისტრატორს შეიძლია ვებ6სერვერის კონფიგურირების დროს სახელი და ვერსიის ნომერი შეცვალოს, რომ ჰაკერი შეცდომაში შეიყვანოს.

# HTTP HEAD

```
# telnet www.kijna.com 80
Trying 62.232.8.1...
Connected to www.kijna.com.
Escape character is '^]'.

HEAD / HTTP/1.0

HTTP/1.1 200 OK

Date: Mon, 26 May 2012 14:28:50 GMT
Server: Apache/1.3.27 (Unix) Debian GNU/Linux PHP/4.3.2
Connection: close
Content-Type: text/html; charset=iso-8859-1
```

# HTTP OPTIONS

```
telnet www.kijna.com 80

Trying 62.232.8.1...
Connected to www.kijna.com.
Escape character is '^]'.

OPTIONS / HTTP/1.0

HTTP/1.1 200 OK

Date: Mon, 26 May 2012 14:29:55 GMT
Server: Apache/1.3.27 (Unix) Debian GNU/Linux PHP/4.3.2
Content-Length: 0
Allow: GET, HEAD, OPTIONS, TRACE
Connection: close
```

გავიგეთ რომ ვებ-სერვერია Apache/1.3.27, ოპერაციუ სისტემაა ლინუქსი Debian, PHP-ს ვერსიაა 4.3.2.

რომ ყოფილიყო mod_ssl, ეს გვანიშნებდა რომ საიტი SSL-ს იყენებს.

# HTTP HEAD და HTTP OPTIONS SSL ტუნელით

როცა ვებ-სერვერის ქსელური მოძრაობა დაშიფრულია SSL-ით იგი გაშვებულია TCP 443 პორტზე. ამ შემთხვევაში რომ დაადგინო ვებ-სერვერის ვერსია ჯერ უნდა შექმნა SSL ტუნელი, ამ საქმისთვისაა ხელსაწყო stunnel.


stunnel.conf ფაილი რომელიც ქმნის SSL ტუნელს secure.example.com:443-თან კავშირისთვის და შემდეგ ელოდება დაუშიფრავ ტრეფიკს ლოკალურ პორტ 80-ზე:

```
client=yes

verify=0

[psuedo-https]

accept  = 80

connect = secure.example.com:443

TIMEOUTclose = 0
```

ამ ფაილის შექმნის შემდეგ, იგივე პაპკაში სადაც პროგრამა stunnel-ია, დაუკავშირდი შენს ლოკალურ მისამართს 127.0.0.1 80 პორტზე:

```
telnet 127.0.0.1 80

Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.

HEAD / HTTP/1.0


HTTP/1.1 200 OK

Server: lighttpd/1.4.30
Date: Mon, 26 May 2012 16:14:29 GMT
Content-type: text/html
Last-modified: Mon, 19 May 2012 10:32:56 GMT
Content-length: 5437
Accept-ranges: bytes
Connection: close
```

# ვებ-სერვერის ანაბეჭდი

```
# nmap -sV -T4 -F insecure.org

Starting Nmap ( http://nmap.org )
Nmap scan report for insecure.org (74.207.254.18)
Host is up (0.016s latency).
rDNS record for 74.207.254.18: web.insecure.org
Not shown: 95 filtered ports
PORT    STATE  SERVICE  VERSION
22/tcp  open   ssh      OpenSSH 4.3 (protocol 2.0)
25/tcp  open   smtp     Postfix smtpd
80/tcp  open   http     Apache httpd 2.2.3 ((CentOS))
113/tcp closed auth
443/tcp open   ssl/http Apache httpd 2.2.3 ((CentOS))
Service Info: Host:  web.insecure.org

Nmap done: 1 IP address (1 host up) scanned in 14.82 seconds
```
