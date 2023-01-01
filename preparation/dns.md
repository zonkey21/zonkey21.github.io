# თეორია

DNS სერვერები ხმარობენ ორ პორტს ინფორმაციის გადაცემისთვის: UDP პორტ 53 პირდაპირი თხოვნების შესასრულრბლად (დომენის IP მისამართების დადგენა...), TCP პორტ 53 ზონის გადაცემისთვის (zone transfer) და სინქრონიზაციისთვის.

DNS სერვისების სრულად შემოწმებისთვის უნდა გააკეთო:
  * DNS სერვერის ვერსიის დადგენა
  * შეეცადო DNS ზონის გადაცემის განხორციელება რომელიმე ცნობილ დომენებთან
  * ცადო მრავალი უკუღმა-ძებნა (reverse-lookup) დომენის შიდა ქსელში
  * შეამოწმო მონაცემების დახარისხების დროს თუ რამე ბაგი ჩდება

# დომენის ავტორიტეტური DNS სერვერები

DNS სერვერები რომლებსაც აქვთ ავტორიტეტი დანიშნულ საიტზე.

## host


|   პროგრამა   |  host | 
|   ო.ს        |  ლინუქსი (იუნიქსი) |

მაგალითი: რომელი DNS-ები აკონტროლებენ fhm.com-ს?

Whois მეშვეობით უკვე ვნახეთ რომ fhm.com-ის პირველი DNS სერვერი არის dns0.vital-group.net; ამ ინფორ;აციას ჩვენ გამოვიყენებთ როგორც შემდეგ:

```
bash-3.1# host -v -t ns fhm.com dns0.vital-group.net
Trying "fhm.com"
Using domain server:
Name: dns0.vital-group.net
Address: 195.167.255.254#53
Aliases:

;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 21891
;; flags: qr aa rd; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 2

;; QUESTION SECTION:
;fhm.com.                       IN      NS

;; ANSWER SECTION:
fhm.com.                300     IN      NS      dns0.vital-group.net.
fhm.com.                300     IN      NS      dns1.vital-group.net.

;; ADDITIONAL SECTION:
dns0.vital-group.net.   900     IN      A       195.167.255.254
dns1.vital-group.net.   900     IN      A       195.167.177.254

Received 110 bytes from 195.167.255.254#53 in 42 ms
```

ამ ეტპზე ჩვენ დადასტურებით ვიცით თუ რა DNS სერვერები აკონტროლებენ fhm.com-ს და ასევი მითი IP მისამართებები.

## dig


|   პროგრამა   |  dig | 
|   ო.ს        |  ლინუქსი (იუნიქსი) |

```
tutashxia@kali ~> dig hackthissite.org -t ns

; <<>> DiG 9.11.4-2-Debian <<>> hackthissite.org -t ns
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 42893
;; flags: qr rd ra; QUERY: 1, ANSWER: 5, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;hackthissite.org.		IN	NS

;; ANSWER SECTION:
hackthissite.org.	3600	IN	NS	c.ns.buddyns.com.
hackthissite.org.	3600	IN	NS	f.ns.buddyns.com.
hackthissite.org.	3600	IN	NS	g.ns.buddyns.com.
hackthissite.org.	3600	IN	NS	h.ns.buddyns.com.
hackthissite.org.	3600	IN	NS	j.ns.buddyns.com.

;; Query time: 166 msec
;; SERVER: 2a01:e00::1#53(2a01:e00::1)
;; WHEN: Sun Jan 20 16:41:20 CET 2019
;; MSG SIZE  rcvd: 139
```

# DNS-ს მინიშნების ანალიზი

|   მინიშნების ტიპი   | აღწერა | 
|   SOA               | Start Of Authority |
|   NS                | Name Server        |
|   A                 | IPv4 Address       |
|   MX                | Mail eXchange      |
|   PTR               | PoinTR             |
|   AAAA              | IPv6 Address       |
|   CNAME             | Canonical NAME     |


# უკუღმა ძებნა ======

თუ host პროგრამას მიუნიშნებ დომენის სახელს მაშინ ამ ასეთ ტიპის ძიებას ქვია forward lookup, თუ მიუნიშნებ IP მისამართს ამას ქვია reverse lookup.

```
tutashxia@kali ~> host 2a00:1450:4007:808::200e
e.0.0.2.0.0.0.0.0.0.0.0.0.0.0.0.8.0.8.0.7.0.0.4.0.5.4.1.0.0.a.2.ip6.arpa domain name pointer par21s17-in-x0e.1e100.net.
```

# ელ-ფოსტის სერვერები

```
bash-3.1# host -v -t mx fhm.com dns0.vital-group.net
Trying "fhm.com"
Using domain server:
Name: dns0.vital-group.net
Address: 195.167.255.254#53
Aliases:

;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 58560
;; flags: qr aa rd; QUERY: 1, ANSWER: 4, AUTHORITY: 2, ADDITIONAL: 2

;; QUESTION SECTION:
;fhm.com.                       IN      MX

;; ANSWER SECTION:
fhm.com.                300     IN      MX      200 emap.com.s200a2.psmtp.com.
fhm.com.                300     IN      MX      300 emap.com.s200b1.psmtp.com.
fhm.com.                300     IN      MX      400 emap.com.s200b2.psmtp.com.
fhm.com.                300     IN      MX      100 emap.com.s200a1.psmtp.com.

;; AUTHORITY SECTION:
fhm.com.                300     IN      NS      dns1.vital-group.net.
fhm.com.                300     IN      NS      dns0.vital-group.net.

;; ADDITIONAL SECTION:
dns0.vital-group.net.   900     IN      A       195.167.255.254
dns1.vital-group.net.   900     IN      A       195.167.177.254

Received 244 bytes from 195.167.255.254#53 in 44 ms
```

# მოპოვნილი ინფორმაციების დადასტურება

პარამეტრი a (-a, ანუ all) გულისხმობს რომ ყველა ინფორმაცია მოიტანოს.

```
bash-3.1# host -a fhm.com dns0.vital-group.net
Trying "fhm.com"
Using domain server:
Name: dns0.vital-group.net
Address: 195.167.255.254#53
Aliases:

;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 30262
;; flags: qr aa rd; QUERY: 1, ANSWER: 8, AUTHORITY: 0, ADDITIONAL: 2

;; QUESTION SECTION:
;fhm.com.                       IN      ANY

;; ANSWER SECTION:
fhm.com.                300     IN      A       195.69.154.45
fhm.com.                300     IN      SOA     dns0.vital-group.net. hostmaster.fhm.com. 2009012201 300 7200 604800 300
fhm.com.                300     IN      NS      dns1.vital-group.net.
fhm.com.                300     IN      NS      dns0.vital-group.net.
fhm.com.                300     IN      MX      300 emap.com.s200b1.psmtp.com.
fhm.com.                300     IN      MX      400 emap.com.s200b2.psmtp.com.
fhm.com.                300     IN      MX      100 emap.com.s200a1.psmtp.com.
fhm.com.                300     IN      MX      200 emap.com.s200a2.psmtp.com.

;; ADDITIONAL SECTION:
dns0.vital-group.net.   900     IN      A       195.167.255.254
dns1.vital-group.net.   900     IN      A       195.167.177.254

Received 307 bytes from 195.167.255.254#53 in 44 ms
```

პროგრამა host-ი ეხმაურება DNS სერვერებს რომლებიც არიან აღნიშნნული /etc/resolv.conf ფაილში. თუ გსურს სხვა DNS სერვერის გამოყენება მაშინ host პროგრამას პარამეტრის მეშვეობით უნდა მიუნიშნო.

# DNS-ის ყოველივე მონაცემის ნახვა

```
tutashxia@kali ~> dig hackthissite.org -t any
;; Connection to 192.168.1.254#53(192.168.1.254) for hackthissite.org failed: connection refused.

; <<>> DiG 9.11.4-2-Debian <<>> hackthissite.org -t any
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 15192
;; flags: qr rd ra; QUERY: 1, ANSWER: 25, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;hackthissite.org.		IN	ANY

;; ANSWER SECTION:
hackthissite.org.	3600	IN	SOA	c.ns.buddyns.com. admin.hackthissite.org. 2018123006 3600 900 604800 86400
hackthissite.org.	3600	IN	MX	10 aspmx.l.google.com.
hackthissite.org.	3600	IN	MX	20 alt1.aspmx.l.google.com.
hackthissite.org.	3600	IN	MX	20 alt2.aspmx.l.google.com.
hackthissite.org.	3600	IN	MX	30 aspmx2.googlemail.com.
hackthissite.org.	3600	IN	MX	30 aspmx3.googlemail.com.
hackthissite.org.	3600	IN	MX	30 aspmx4.googlemail.com.
hackthissite.org.	3600	IN	MX	30 aspmx5.googlemail.com.
hackthissite.org.	3600	IN	NS	c.ns.buddyns.com.
hackthissite.org.	3600	IN	NS	f.ns.buddyns.com.
hackthissite.org.	3600	IN	NS	g.ns.buddyns.com.
hackthissite.org.	3600	IN	NS	h.ns.buddyns.com.
hackthissite.org.	3600	IN	NS	j.ns.buddyns.com.
hackthissite.org.	3600	IN	SPF	"v=spf1 a mx ptr ip4:198.148.81.135 ip4:137.74.187.96 ip4:137.74.187.97 ip4:137.74.187.98 a:mail.hackthissite.org include:aspmx.googlemail.com ~all"
hackthissite.org.	3600	IN	TXT	"v=spf1 a mx ptr ip4:198.148.81.135 ip4:137.74.187.96 ip4:137.74.187.97 ip4:137.74.187.98 a:mail.hackthissite.org include:aspmx.googlemail.com ~all"
hackthissite.org.	3600	IN	A	137.74.187.103
hackthissite.org.	3600	IN	A	137.74.187.101
hackthissite.org.	3600	IN	A	137.74.187.100
hackthissite.org.	3600	IN	A	137.74.187.104
hackthissite.org.	3600	IN	A	137.74.187.102
hackthissite.org.	3600	IN	AAAA	2001:41d0:8:ccd8:137:74:187:100
hackthissite.org.	3600	IN	AAAA	2001:41d0:8:ccd8:137:74:187:102
hackthissite.org.	3600	IN	AAAA	2001:41d0:8:ccd8:137:74:187:103
hackthissite.org.	3600	IN	AAAA	2001:41d0:8:ccd8:137:74:187:101
hackthissite.org.	3600	IN	AAAA	2001:41d0:8:ccd8:137:74:187:104

;; Query time: 164 msec
;; SERVER: 2a01:e00::2#53(2a01:e00::2)
;; WHEN: Sun Jan 20 16:45:09 CET 2019
;; MSG SIZE  rcvd: 895 
```

# dnsenum

dnsenum-ი ადგენს დომენის კომპიუტერების IP მისამართებს, DNS სერვერვს და MX (მაილის) სერვერს.
და კიდევ:
 * google.com-ის მეშვეობით ნახულობს დომენის და ქვედადომენების სახელწოდებებს.
 * Brute force-ის მეშვეობით ადგენს ქვედადომენების სახელებს, მას მოყვება ორი ფაილი: dns.txt რომელიც შეიცავს 1480 ქვედადომენის სახელს და dns-big.txt კი 266930 ქვედადომენის სახელს
 * C კლასის ქსელების შემთხვევაში ანხორციელებს Whois-ს ძიებას და ადგენს ქსელში რამდენი IP-ეა.
 * უკუღმა ძებმა მთლიანი ქსელის დონეზე.
 * ხმარობს სრედებს რომ ჭქარა იმოქმედოს.

```
tutashxia@kali ~> dnsenum hackthissite.org
Smartmatch is experimental at /usr/bin/dnsenum line 698.
Smartmatch is experimental at /usr/bin/dnsenum line 698.
dnsenum VERSION:1.2.4

-----   hackthissite.org   -----


Host's addresses:
__________________

hackthissite.org.                        3600     IN    A        137.74.187.100
hackthissite.org.                        3600     IN    A        137.74.187.104
hackthissite.org.                        3600     IN    A        137.74.187.102
hackthissite.org.                        3600     IN    A        137.74.187.101
hackthissite.org.                        3600     IN    A        137.74.187.103


Name Servers:
______________

c.ns.buddyns.com.                        10800    IN    A        88.198.106.11
h.ns.buddyns.com.                        2630     IN    A        119.252.20.56
g.ns.buddyns.com.                        1800     IN    A        107.181.178.180
j.ns.buddyns.com.                        3120     IN    A        185.34.136.178
f.ns.buddyns.com.                        1800     IN    A        103.6.87.125


Mail (MX) Servers:
___________________

aspmx.l.google.com.                      562      IN    A        173.194.76.26
alt1.aspmx.l.google.com.                 952      IN    A        74.125.131.26
alt2.aspmx.l.google.com.                 1773     IN    A        74.125.24.27
aspmx2.googlemail.com.                   952      IN    A        74.125.131.26
aspmx3.googlemail.com.                   731      IN    A        74.125.24.27
aspmx4.googlemail.com.                   1800     IN    A        108.177.125.26
aspmx5.googlemail.com.                   1773     IN    A        74.125.195.27


Trying Zone Transfers and getting Bind Versions:
_________________________________________________


Trying Zone Transfer for hackthissite.org on c.ns.buddyns.com ... 
AXFR record query failed: corrupt packet

Trying Zone Transfer for hackthissite.org on h.ns.buddyns.com ... 
AXFR record query failed: corrupt packet

Trying Zone Transfer for hackthissite.org on g.ns.buddyns.com ... 
AXFR record query failed: corrupt packet

Trying Zone Transfer for hackthissite.org on j.ns.buddyns.com ... 
AXFR record query failed: corrupt packet

Trying Zone Transfer for hackthissite.org on f.ns.buddyns.com ... 
AXFR record query failed: corrupt packet

brute force file not specified, bay.
```

ვიხმაროთ google.com ქევდადომენების მოსაძებმად (-p გუგლის_გვერდის_რაოდენობა), -s ქევდადომენების_რაოდენობა, --threads სრედის_რაოდენობა.

```
tutashxia@kali ~> dnsenum -p 5 -s 10 --threads 3 hackthissite.org
```

# fierce


fierce-ი ადგენს IP მისამართებს და სეხელწოდებებს სხვადასხვა მეთოდით. ჯერ ხმარობს თქვენი სისტემის DNS-ს შემდეგ კი დამიზნებულ DNS-ს. აგრეთვე შეუძლოა რომ გამოიყენოს სიტყვების სია (wordlists).

```
tutashxia@kali ~> fierce -dns hackthissite.org -threads 3
DNS Servers for hackthissite.org:
	c.ns.buddyns.com
	f.ns.buddyns.com
	g.ns.buddyns.com
	h.ns.buddyns.com
	j.ns.buddyns.com

Trying zone transfer first...
	Testing c.ns.buddyns.com
		Request timed out or transfer not allowed.
	Testing f.ns.buddyns.com
		Request timed out or transfer not allowed.
	Testing g.ns.buddyns.com
		Request timed out or transfer not allowed.
	Testing h.ns.buddyns.com
		Request timed out or transfer not allowed.
	Testing j.ns.buddyns.com
		Request timed out or transfer not allowed.

Unsuccessful in zone transfer (it was worth a shot)
Okay, trying the good old fashioned way... brute force

Checking for wildcard DNS...
Nope. Good.
Now performing 2280 test(s)...
198.148.81.160	admin.hackthissite.org
137.74.187.100	hp.hackthissite.org
137.74.187.134	mirror.hackthissite.org
137.74.187.133	mirror.hackthissite.org
137.74.187.150	irc.hackthissite.org
137.74.187.137	legal.hackthissite.org
198.148.81.188	ns1.hackthissite.org
198.148.81.189	ns2.hackthissite.org
198.148.81.135	mail.hackthissite.org
137.74.187.135	stats.hackthissite.org
137.74.187.136	stats.hackthissite.org
198.148.81.167	tor.hackthissite.org
185.24.222.13	irc.hackthissite.org
137.74.187.104	www.hackthissite.org
137.74.187.100	www.hackthissite.org
137.74.187.102	www.hackthissite.org
137.74.187.103	www.hackthissite.org
137.74.187.101	www.hackthissite.org
198.148.81.170	radio.hackthissite.org
137.74.187.123	pi.hackthissite.org

Subnets found (may want to probe here using nmap or unicornscan):
	137.74.187.0-255 : 13 hostnames found.
	185.24.222.0-255 : 1 hostnames found.
	198.148.81.0-255 : 6 hostnames found.

Done with Fierce scan: http://ha.ckers.org/fierce/
Found 20 entries.

Have a nice day.
```

# DMitry

DMitry (Deepmagic Information Gathering Tool). მას შეუძლია:

  * Whois ინფორმაციის მოპოება.
  * საიტის შესახებ ინფორმაციის მოპოება netcraft.com-ის მეშვეობით.
  * ქვედადომენების აღმოჩენა.
  * დომენის ელ.ფოსტების აღმოჩენა.
  * პორტების სკანირება.




```
tutashxia@kali ~> dmitry -iwnse hackthissite.org
Deepmagic Information Gathering Tool
"There be some deep magic going on"

HostIP:137.74.187.104
HostName:hackthissite.org

Gathered Inet-whois information for 137.74.187.104
--------------------------------------------------


inetnum:        137.74.187.96 - 137.74.187.127
netname:        OVH_113911647
descr:          OVH Static IP
country:        NL
org:            ORG-SH80-RIPE
admin-c:        OTC7-RIPE
tech-c:         OTC7-RIPE
status:         ASSIGNED PA
mnt-by:         OVH-MNT
created:        2016-08-25T08:53:54Z
last-modified:  2016-08-25T08:53:54Z
source:         RIPE

organisation:   ORG-SH80-RIPE
org-name:       Staff HackThisSite
org-type:       OTHER
address:        Stadtmitte 1
address:        10117 Berlin
address:        DE
phone:          +49.151011011
mnt-ref:        OVH-MNT
mnt-by:         OVH-MNT
created:        2016-07-28T19:32:04Z
last-modified:  2017-10-30T16:51:28Z
source:         RIPE # Filtered

role:           OVH NL Technical Contact
address:        OVH BV
address:        Corkstraat 46
address:        3047 AC Rotterdam
address:        The Netherlands
admin-c:        OK217-RIPE
tech-c:         GM84-RIPE
nic-hdl:        OTC7-RIPE
abuse-mailbox:  abuse@ovh.net
mnt-by:         OVH-MNT
created:        2009-03-18T15:51:01Z
last-modified:  2009-03-18T15:51:01Z
source:         RIPE # Filtered

% Information related to '137.74.0.0/16AS16276'

route:          137.74.0.0/16
origin:         AS16276
descr:          OVH
mnt-by:         OVH-MNT
created:        2016-07-15T10:03:53Z
last-modified:  2016-07-15T10:03:53Z
source:         RIPE

% This query was served by the RIPE Database Query Service version 1.92.6 (ANGUS)



Gathered Inic-whois information for hackthissite.org
----------------------------------------------------
Domain Name: HACKTHISSITE.ORG
Registry Domain ID: D99641092-LROR
Registrar WHOIS Server: whois.enom.com
Registrar URL: http://www.enom.com
Updated Date: 2019-01-14T03:31:05Z
Creation Date: 2003-08-10T15:01:25Z
Registry Expiry Date: 2019-08-10T15:01:25Z
Registrar Registration Expiration Date:
Registrar: eNom, Inc.
Registrar IANA ID: 48
Registrar Abuse Contact Email: abuse@enom.com
Registrar Abuse Contact Phone: +1.4252982646
Reseller:
Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
Registrant Organization: Whois Privacy Protection Service, Inc.
Registrant State/Province: WA
Registrant Country: US
Name Server: C.NS.BUDDYNS.COM
Name Server: F.NS.BUDDYNS.COM
Name Server: G.NS.BUDDYNS.COM
Name Server: H.NS.BUDDYNS.COM
Name Server: J.NS.BUDDYNS.COM
DNSSEC: unsigned
URL of the ICANN Whois Inaccuracy Complaint Form https://www.icann.org/wicf/)
>>> Last update of WHOIS database: 2019-01-20T23:16:24Z <<<

For more information on Whois status codes, please visit https://icann.org/epp
```

# ყოველივე დომენის კომპიუტერის IP მისამართის დადგენა (ზონის გადაცემა)

უმრავლეს საიტებს აქვთ ორი DNS სერვერი, პირველი სერვერი არის "პატრონი", მეორე კი "მონა".
თუ პირველი DNS სერვერი გაფუჭდა, მეორე DNS სერვერი ასრულებს DNS-ის ფუნქციას.

ზონის გადაცემა (zone transfer) ნიშნავს რომ პირველი DNS სერვერი მეორე DNS სერვერს გადასცემს მონაცემებს რომლებსაც იგ ფლობს რახან მეორე სერვერს შეეძლოს DNS-ის ფუნქციის შესრულება თუ პირველი სეერვერი გაფუჭდა. ეს მონაცემები შეიცავს ყოველივე დომენის კომპიუტერის IP მისამართს.

თუ საიტის ადმინისტრატორი კარგი დონისაა, ამ ტექნიკის ინტერნეტიდან განხორციელება არ იმოქმედებს, მაგრამ, თუ ცუდი დონისაა, გამოვა ;)

## host

ცადე ასე (რათქმაუნდა დამიზნული საიტით უნდა შეცვალო):

```
host -l fhm.com dns0.vital-group.net
```

თუ პირველი DNS სერვერზე ცდით არაფერი რეზულტატი მიიყე, ცადე მეორე DNS სერვერით, იმიტომ რომ მეორე DNS სერვერი ხშირად უფრო ნაკლებად კარგად კონფიგურილებულია (ნაკლებად დაცულია).

## dnswalk

```
root@bt:/pentest/enumeration/dns/dnswalk# ./dnswalk reactfeminism.de.
Checking reactfeminism.de.
Getting zone transfer of reactfeminism.de. from ns1.hans.hosteurope.de...failed
FAIL: Zone transfer of reactfeminism.de. from ns1.hans.hosteurope.de failed: truncated zone transfer
Getting zone transfer of reactfeminism.de. from ns2.hans.hosteurope.de...failed
FAIL: Zone transfer of reactfeminism.de. from ns2.hans.hosteurope.de failed: truncated zone transfer
BAD: All zone transfer attempts of reactfeminism.de. failed!
2 failures, 0 warnings, 1 errors.
```

## dig

```
root@bt:~$ dig @sk.s5.ans1.ns98.ztomy.com axfr
```


## სხვა გზა შიდა კომპიუტერების IP მისამართების მოპოვმისათვის

თუ ზონის გადაცემა არ გამოდის, შეიძლიება მაინც მოიპოვი შიდა კომპიუტერების IP მისამართი.
სულ არ გამოდის ეს ტექნიკა მაგრამ მაინც.

```
bash-3.1# host -a 0.168.192.in-addr.arpa dns0.vital-group.net
Trying "0.168.192.in-addr.arpa"
Using domain server:
Name: dns0.vital-group.net
Address: 195.167.255.254#53
Aliases:

;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 42870
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 13, ADDITIONAL: 0

;; QUESTION SECTION:
;0.168.192.in-addr.arpa.                IN      ANY

;; AUTHORITY SECTION:
.                       3600000 IN      NS      B.ROOT-SERVERS.NET.
.                       3600000 IN      NS      C.ROOT-SERVERS.NET.
.                       3600000 IN      NS      D.ROOT-SERVERS.NET.
.                       3600000 IN      NS      E.ROOT-SERVERS.NET.
.                       3600000 IN      NS      F.ROOT-SERVERS.NET.
.                       3600000 IN      NS      G.ROOT-SERVERS.NET.
.                       3600000 IN      NS      H.ROOT-SERVERS.NET.
.                       3600000 IN      NS      I.ROOT-SERVERS.NET.
.                       3600000 IN      NS      J.ROOT-SERVERS.NET.
.                       3600000 IN      NS      K.ROOT-SERVERS.NET.
.                       3600000 IN      NS      L.ROOT-SERVERS.NET.
.                       3600000 IN      NS      M.ROOT-SERVERS.NET.
.                       3600000 IN      NS      A.ROOT-SERVERS.NET.

Received 251 bytes from 195.167.255.254#53 in 41 ms
```

ამ შემთხვევაში შიდა კომპიუტერების IP მისამართებები ვერ მოვიპოვეთ, ეგ მისამართები წესი ლოკალური ქსელის მისამართებისავით არიან (192.168.0.1, 192.168.0.2 და ეგეთები).

# nslookup

| პროგრამა   |	nslookup  |
| ო.ს            |	ლინუქსი (იუნიქსი) ან ვინდოუსი   |

nslookup-ი შეეკითხება DNS რომ გაიგოს თუ რა სახელი არის დაკევშიებული IP მისამართან.

```
bash-3.1# nslookup 82.56.33.56
Server:         212.27.40.241
Address:        212.27.40.241#53

Non-authoritative answer:
56.33.56.82.in-addr.arpa name = host56-33-dynamic.56-82-r.retail.telecomitalia.it.

Authoritative answers can be found from:
```

ამ შემთხვევაში, nslookup გვეუბნება რო 82.56.33.56 არის host56-33-dynamic.56-82-r.retail.telecomitalia.it

```
bash-3.1# nslookup www.google.com
Server:         212.27.40.241
Address:        212.27.40.241#53

Non-authoritative answer:
www.google.com  canonical name = www.l.google.com.
Name:   www.l.google.com
Address: 74.125.39.147
Name:   www.l.google.com
Address: 74.125.39.99
Name:   www.l.google.com
Address: 74.125.39.104
Name:   www.l.google.com
Address: 74.125.39.103
```

როგორც ხედავთ, www.google.com-ს რამოდენიმე IP მისამართი აქვს.

# IPv6 მისამართების დადგენა

dnsdict6-ს შეუძლია ჩამოთვალოს DNS-ში შეტანილი IPv6 მისამართები (AAAA მონაცემები), ეს პრაქტიკულია ქვედომენების აღმოსაჩენად რომლებიც საჯაროთ ნაჩვენები არ იქნებოდა, მაგრამ რომლებიც ჩაწერილია DNS-ში. ხშირად ეს დომენები დავიწყებულია და შესაძლოა საინტერესო გზა გახდეს ექსპლოიტების საცდელად. dnsdict6-ს შეუძლია ლექსიკონის ხმარება DNS-ის მონაცემების სახელების გამოსაცნობად.

**მაგალითი:** გავიგოთ google.com-ის IPv6 ქვედომენები 3 სრედის მეშვეობით.

```
root@bt:~# dnsdict6 -t 3 google.com
Starting enumerating google.com. - creating 3 threads for 798 words...
Estimated time to completion: 1 to 3 minutes

www.google.com. => 2a00:1450:400c:c01::67
mail.google.com. => 2a00:1450:4007:801::1016
ipv6.google.com. => 2a00:1450:400c:c01::67
news.google.com. => 2a00:1450:4007:801::1007
blog.google.com. => 2a00:1450:400c:c01::bf
web.google.com. => 2a00:1450:4007:801::1009
support.google.com. => 2a00:1450:4007:801::1001
wap.google.com. => 2a00:1450:4007:803::1007
m.google.com. => 2a00:1450:400c:c01::c1
images.google.com. => 2a00:1450:4007:803::1009
search.google.com. => 2a00:1450:4007:802::1003
calendar.google.com. => 2a00:1450:4007:801::1009
photo.google.com. => 2a00:1450:400c:c01::5d
download.google.com. => 2a00:1450:4007:804::1012
enterprise.google.com. => 2a00:1450:4007:802::1003
mars.google.com. => 2a00:1450:4007:803::1007
email.google.com. => 2a00:1450:4007:801::1009
id.google.com. => 2a00:1450:4007:804::1018
docs.google.com. => 2a00:1450:4007:801::1005
sms.google.com. => 2a00:1450:4007:802::1000
earth.google.com. => 2a00:1450:4007:803::1002
dl.google.com. => 2a00:1450:400c:c01::be
music.google.com. => 2a00:1450:4007:804::1006
apps.google.com. => 2a00:1450:4007:803::1007
map.google.com. => 2a00:1450:4007:803::1000
photos.google.com. => 2a00:1450:400c:c01::5d
moon.google.com. => 2a00:1450:4007:804::1003
downloads.google.com. => 2a00:1450:4007:803::1013
directory.google.com. => 2a00:1450:4007:801::1001
maps.google.com. => 2a00:1450:4007:803::1000
research.google.com. => 2a00:1450:4007:804::1006
code.google.com. => 2a00:1450:4007:804::100e
ap.google.com. => 2a00:1450:4007:804::1014
jobs.google.com. => 2a00:1450:4007:804::1006
local.google.com. => 2a00:1450:4007:803::1007

Found 35 domain names and 23 unique ipv6 addresss for google.com.
```

**მაგალითი:** გავიგოთ victoriassecret.com-ის IPv4 და IPv6 ქვედომენები და თან დაბეჭდოს IPv6-ის ინფორმაცია NS და MX record-ის შესახებ 3 სრედის მეშვეობით.

```
root@bt:~# dnsdict6 -4 -d -t 3 victoriassecret.com
NS of victoriassecret.com. is usw3.akam.net. -> 63.235.29.201
NS of victoriassecret.com. is ns1-107.akam.net. -> 193.108.91.107
NS of victoriassecret.com. is usw4.akam.net. -> 96.17.144.195
NS of victoriassecret.com. is use1.akam.net. -> 208.44.108.136
NS of victoriassecret.com. is eur1.akam.net. -> 213.155.153.132
NS of victoriassecret.com. is usw1.akam.net. -> 204.245.152.68
NS of victoriassecret.com. is ns1-108.akam.net. -> 193.108.91.108
NS of victoriassecret.com. is usc1.akam.net. -> 64.208.249.147
No IPv6 address for NS entries found in DNS for domain victoriassecret.com.
MX of victoriassecret.com. is cluster2a.us.messagelabs.com. -> 216.82.251.230
MX of victoriassecret.com. is cluster2a.us.messagelabs.com. -> 85.158.139.103
MX of victoriassecret.com. is cluster2.us.messagelabs.com. -> 216.82.254.67
MX of victoriassecret.com. is cluster2.us.messagelabs.com. -> 216.82.254.83
MX of victoriassecret.com. is cluster2.us.messagelabs.com. -> 216.82.254.163
MX of victoriassecret.com. is cluster2.us.messagelabs.com. -> 216.82.254.179
MX of victoriassecret.com. is cluster2.us.messagelabs.com. -> 216.82.241.131
MX of victoriassecret.com. is cluster2.us.messagelabs.com. -> 216.82.249.3
MX of victoriassecret.com. is cluster2.us.messagelabs.com. -> 216.82.249.179
MX of victoriassecret.com. is cluster2.us.messagelabs.com. -> 216.82.249.211
No IPv6 address for MX entries found in DNS for domain victoriassecret.com.
Starting enumerating victoriassecret.com. - creating 3 threads for 798 words...
Estimated time to completion: 1 to 3 minutes

www.victoriassecret.com. -> 209.170.115.120
www.victoriassecret.com. -> 209.170.115.145
mail.victoriassecret.com. -> 206.175.140.13
www2.victoriassecret.com. -> 209.170.115.130
www2.victoriassecret.com. -> 209.170.115.154
secure.victoriassecret.com. -> 96.6.166.89
search.victoriassecret.com. -> 208.67.43.132
media.victoriassecret.com. -> 72.246.94.173
media.victoriassecret.com. -> 72.246.94.142
email.victoriassecret.com. -> 184.84.247.48
email.victoriassecret.com. -> 184.84.247.33
t.victoriassecret.com. -> 63.72.208.100

Found 8 domain names, 12 unique ipv4 and 0 unique ipv6 addresses for victoriassecret.com.
```

# სხვადასხვა ინფორმაციის მოგროვება საიტის DNS-ის შესახებ

## dnsenum

dnsenum-ი არის პერლში დაწერილი პროგრამა რომელიც აგროვებს სხვადასხვა ინფორმაცია მოცემული საიტის DNS-ების შესახებ. ინფორმაციები როგორც:
  * A records (სერვერის მისამართი)
  * DNS-ი
  * MX records (წერილების სერვერები)
  * BIND-ის ვერსია (DNS-ის სერვერია)
  * დაუფიქსირებულ ქვედომენებს (ეძებს ლექსიკონის მეშვეობით)
  * Reverse DNS lookup ანუ უკუღმა მოძებნა, მხოლოდ C კლასის ქსელებისთვის 

**მაგალითი:** გავიგოთ ინფორმაციები reactfeminism.de-ის DNS-ის შესახებ.

```
root@bt:/pentest/enumeration/dns/dnsenum# ./dnsenum.pl -enum -f dns.txt -update a -r reactfeminism.de 
dnsenum.pl VERSION:1.2.2
Warning: can't load Net::Whois::IP module, whois queries disabled.

-----   reactfeminism.de   -----


Host's addresses:
__________________

reactfeminism.de                         86400    IN    A        83.169.45.11


Name Servers:
______________

ns1.hans.hosteurope.de                   500      IN    A        217.115.143.140
ns2.hans.hosteurope.de                   949      IN    A        80.237.128.10


Mail (MX) Servers:
___________________

mx0.reactfeminism.de                     86400    IN    A        80.237.138.5


Trying Zone Transfers and getting Bind Versions:
_________________________________________________


Trying Zone Transfer for reactfeminism.de on ns1.hans.hosteurope.de ... 
AXFR record query failed: FORMERR
Unable to obtain Server Version for ns1.hans.hosteurope.de : FORMERR

Trying Zone Transfer for reactfeminism.de on ns2.hans.hosteurope.de ... 
AXFR record query failed: FORMERR
Unable to obtain Server Version for ns2.hans.hosteurope.de : FORMERR


Scraping reactfeminism.de subdomains from Google:
__________________________________________________


 ----   Google search page: 1   ---- 


Google Results:
________________

  perhaps Google is blocking our queries.
 Check manually.


Brute forcing with dns.txt:
____________________________

ftp.reactfeminism.de                     86400    IN    A        83.169.45.11
mail.reactfeminism.de                    86400    IN    A        80.237.132.230
www.reactfeminism.de                     86400    IN    A        83.169.45.11


Performing recursion:
______________________


 ---- Checking subdomains NS records ----

  Can't perform recursion no NS records.


reactfeminism.de class C netranges:
____________________________________

 80.237.132.0/24
 80.237.138.0/24
 83.169.45.0/24


Performing reverse lookup on 768 ip addresses:
_______________________________________________


0 results out of 768 IP addresses.


reactfeminism.de ip blocks:
____________________________


done.
```


## dnsmap

dnsmap-ი ეძებს ქვედომენებს, მას მოყვება თავისივე ლექსიკონი მაგრამ შეგიძლიათ რომ თქევინივე მიუთითოთ რომ ქვედომენებიბს სახელები გამოიცნოს. რეზულტატების შენახვა შეუძლია .txt და .cvs-ში. 

**გამოყენება:**

```
dnsmap 0.30 - DNS Network Mapper by pagvac (gnucitizen.org)

usage: dnsmap <target-domain> [options]
options:
-w <wordlist-file>
-r <regular-results-file>
-c <csv-results-file>
-d <delay-millisecs>
-i <ips-to-ignore> (useful if you're obtaining false positives)

e.g.:
dnsmap target-domain.foo
dnsmap target-domain.foo -w yourwordlist.txt -r /tmp/domainbf_results.txt
dnsmap target-fomain.foo -r /tmp/ -d 3000
dnsmap target-fomain.foo -r ./domainbf_results.txt
```

**მაგალითი:**

```
root@bt:/pentest/enumeration/dns/dnsmap# ./dnsmap reactfeminism.de
dnsmap 0.30 - DNS Network Mapper by pagvac (gnucitizen.org)

[+] searching (sub)domains for reactfeminism.de using built-in wordlist
[+] using maximum random delay of 10 millisecond(s) between requests

ftp.reactfeminism.de
IP address #1: 83.169.45.11

mail.reactfeminism.de
IP address #1: 80.237.132.230

mx0.reactfeminism.de
IP address #1: 80.237.138.5

www.reactfeminism.de
IP address #1: 83.169.45.11

[+] 4 (sub)domains and 4 IP address(es) found
[+] completion time: 125 second(s)
```


## dnsrecon

dnsrecon-ი არის პითონში დაწერილი პროგრამა რომელსაც შეუძლია მოიპოვოს DNS მონაცემებიდან შემდეგი ინფორმაცია:
  * IP ბლოკების უკუღმა ძებნა (reverse lookups)
  * უმაღლესი დომენის გაფართოება (top level domain expansion)
  * DNS სერვერის და დომენის უხეში ძალით (bruteforce) სახელწოდების გამოცნობა  
  * A, NS, SOA და MX მონაცემის მოძებნა
  * ყოველოვე ნაპოვნი NS სერვერზე ზონის გადატანა (zone transfer)
  * ეძებს SRV მონაცემებს

**მაგალითი:** გამოვიცნოთ ქვედომენები უხეში ძალით (-t brt) და ამისთვის გამოვიყენოთ ლექსიკონი namelist.txt.

```
root@bt:/pentest/enumeration/dns/dnsrecon# ./dnsrecon.py -t brt -d reactfeminism.de -D namelist.txt 
[*] Performing host and subdomain brute force against reactfeminism.de
[*]	A ftp.reactfeminism.de 83.169.45.11
[*]	A mail.reactfeminism.de 80.237.132.230
[*]	A www.reactfeminism.de 83.169.45.11
```

## fierce

fierce-ი არის პერლში დაწერილი პროგრამა რომელიც ასკანირებს არა მომიჯნავე IP მისამართებს. შემდეგ ად აღმოჩენული IP მისამართები შეგვიძლია მივაწოდოთ სხვა პროგრამებს nmap, nessus, nikto, და სხვა.

**გამოყენება:**

```
fierce.pl (C) Copywrite 2006,2007 - By RSnake at http://ha.ckers.org/fierce/

	Usage: perl fierce.pl [-dns example.com] [OPTIONS]

Overview:
	Fierce is a semi-lightweight scanner that helps locate non-contiguous
	IP space and hostnames against specified domains.  It's really meant
	as a pre-cursor to nmap, unicornscan, nessus, nikto, etc, since all 
	of those require that you already know what IP space you are looking 
	for.  This does not perform exploitation and does not scan the whole 
	internet indiscriminately.  It is meant specifically to locate likely 
	targets both inside and outside a corporate network.  Because it uses 
	DNS primarily you will often find mis-configured networks that leak 
	internal address space. That's especially useful in targeted malware.

Options:
	-connect	Attempt to make http connections to any non RFC1918
		(public) addresses.  This will output the return headers but
		be warned, this could take a long time against a company with
		many targets, depending on network/machine lag.  I wouldn't
		recommend doing this unless it's a small company or you have a
		lot of free time on your hands (could take hours-days).  
		Inside the file specified the text "Host:\n" will be replaced
		by the host specified. Usage:

	perl fierce.pl -dns example.com -connect headers.txt

	-delay		The number of seconds to wait between lookups.
	-dns		The domain you would like scanned.
	-dnsfile  	Use DNS servers provided by a file (one per line) for
                reverse lookups (brute force).
	-dnsserver	Use a particular DNS server for reverse lookups 
		(probably should be the DNS server of the target).  Fierce
		uses your DNS server for the initial SOA query and then uses
		the target's DNS server for all additional queries by default.
	-file		A file you would like to output to be logged to.
	-fulloutput	When combined with -connect this will output everything
		the webserver sends back, not just the HTTP headers.
	-help		This screen.
	-nopattern	Don't use a search pattern when looking for nearby
		hosts.  Instead dump everything.  This is really noisy but
		is useful for finding other domains that spammers might be
		using.  It will also give you lots of false positives, 
		especially on large domains.
	-range		Scan an internal IP range (must be combined with 
		-dnsserver).  Note, that this does not support a pattern
		and will simply output anything it finds.  Usage:

	perl fierce.pl -range 111.222.333.0-255 -dnsserver ns1.example.co

	-search		Search list.  When fierce attempts to traverse up and
		down ipspace it may encounter other servers within other
		domains that may belong to the same company.  If you supply a 
		comma delimited list to fierce it will report anything found.
		This is especially useful if the corporate servers are named
		different from the public facing website.  Usage:

	perl fierce.pl -dns examplecompany.com -search corpcompany,blahcompany 

		Note that using search could also greatly expand the number of
		hosts found, as it will continue to traverse once it locates
		servers that you specified in your search list.  The more the
		better.
	-suppress	Suppress all TTY output (when combined with -file).
	-tcptimeout	Specify a different timeout (default 10 seconds).  You
		may want to increase this if the DNS server you are querying
		is slow or has a lot of network lag.
	-threads  Specify how many threads to use while scanning (default
	  is single threaded).
	-traverse	Specify a number of IPs above and below whatever IP you
		have found to look for nearby IPs.  Default is 5 above and 
		below.  Traverse will not move into other C blocks.
	-version	Output the version number.
	-wide		Scan the entire class C after finding any matching
		hostnames in that class C.  This generates a lot more traffic
		but can uncover a lot more information.
	-wordlist	Use a seperate wordlist (one word per line).  Usage:

	perl fierce.pl -dns examplecompany.com -wordlist dictionary.txt
```

**მაგალითი:**

```
root@bt:/pentest/enumeration/dns/fierce# ./fierce.pl -dns reactfeminism.de
DNS Servers for reactfeminism.de:
	ns1.hans.hosteurope.de
	ns2.hans.hosteurope.de

Trying zone transfer first...
	Testing ns1.hans.hosteurope.de
		Request timed out or transfer not allowed.
	Testing ns2.hans.hosteurope.de
		Request timed out or transfer not allowed.

Unsuccessful in zone transfer (it was worth a shot)
Okay, trying the good old fashioned way... brute force

Checking for wildcard DNS...
Nope. Good.
Now performing 1895 test(s)...
83.169.45.11	ftp.reactfeminism.de
80.237.132.230	mail.reactfeminism.de
83.169.45.11	www.reactfeminism.de

Subnets found (may want to probe here using nmap or unicornscan):
	80.237.132.0-255 : 1 hostnames found.
	83.169.45.0-255 : 2 hostnames found.

Done with Fierce scan: http://ha.ckers.org/fierce/
Found 3 entries.

Have a nice day.
```


## reverseraider

**გამოყენება:**

```
ReverseRaider domain scanner v0.7.7 - Acri Emanuele (crossbower@gmail.com)
Usage:
  reverseraider -d domain | -r range [options]
Options:
  -r	range of ipv4 or ipv6 addresses, for reverse scanning
    	examples: 208.67.1.1-254 or 2001:0DB8::1428:57ab-6344
  -f	file containing lists of ip addresses, for reverse scanning
  -d	domain, for wordlist scanning (example google.com)
  -w	wordlist file (see wordlists directory...)
Extra options:
  -t	requests timeout in seconds
  -P	enable numeric permutation on wordlist (default off)
  -D	nameserver to use (default: resolv.conf)
  -T	use TCP queries instead of UDP queries
  -R	don't set the recursion bit on queries
```


**მაგალითი:**

```
root@bt:/pentest/enumeration/reverseraider# ./reverseraider -d reactfeminism.de -w wordlists/fast.list 
www.reactfeminism.de            	83.169.45.11
mail.reactfeminism.de           	80.237.132.230
www.reactfeminism.de            	83.169.45.11
mail.reactfeminism.de           	80.237.132.230
```


# ავტორიტეტური DNS-ის დადგენა

dnstracer-ი არის პროგრამა რომელიც ადგენს DNS-ი რომელ სხვა DNS-ებს უკავშირდება ძებნის დროს. ამით ვიგებთ რომელია ავტორიტეტული ანუ მთავარი DNS-ი გარკვეული ზონისთვის, და რომელი შუამავალი DNS-ების დაყრდნობით მიახცია ინფორმაციას.


**გამოყენება:**

```
DNSTRACER version 1.8.1 - (c) Edwin Groothuis - http://www.mavetju.org
Usage: dnstracer [options] [host]
	-c: disable local caching, default enabled
	-C: enable negative caching, default disabled
	-o: enable overview of received answers, default disabled
	-q <querytype>: query-type to use for the DNS requests, default A
	-r <retries>: amount of retries for DNS requests, default 3
	-s <server>: use this server for the initial request, default localhost
	             If . is specified, A.ROOT-SERVERS.NET will be used.
	-t <maximum timeout>: Limit time to wait per try
	-v: verbose
	-S <ip address>: use this source address.
	-4: don't query IPv6 servers
```

**მაგალითი:**

```
root@bt:~# dnstracer -v reactfeminism.de
Tracing to reactfeminism.de[a] via 192.168.0.245, maximum of 3 retries
192.168.0.245 (192.168.0.245) IP HEADER
- Destination address:  192.168.0.245
DNS HEADER (send)
- Identifier:           0x316F
- Flags:                0x00 (Q )
- Opcode:               0 (Standard query)
- Return code:          0 (No error)
- Number questions:     1
- Number answer RR:     0
- Number authority RR:  0
- Number additional RR: 0
QUESTIONS (send)
- Queryname:            (13)reactfeminism(2)de
- Type:                 1 (A)
- Class:                1 (Internet)
DNS HEADER (recv)
- Identifier:           0x316F
- Flags:                0x8085 (R RA )
- Opcode:               0 (Standard query)
- Return code:          5 (Refused)
- Number questions:     0
- Number answer RR:     0
- Number authority RR:  0
- Number additional RR: 0
QUESTIONS (recv)
- Queryname:            (1).
- Type:                 0 (unknown)
- Class:                0 (unknown)
```

# DNS სერვერის ვერსიის დადგენა

## dig

სინტაქსი:

```
dig @//dns_სერვერი// version.bind chaos txt
```

დავადგინოთ DNS სერვერის ვერსია, ამ შემთხვევაში თუ სერვერი Bind-ია:

```
root@bt:~$ dig @sk.s5.ans1.ns98.ztomy.com version.bind chaos txt

; <<>> DiG 9.7.0-P1 <<>> @sk.s5.ans1.ns98.ztomy.com version.bind chaos txt
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 26973
;; flags: qr aa rd; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0
;; WARNING: recursion requested but not available

;; QUESTION SECTION:
;version.bind.			CH	TXT

;; ANSWER SECTION:
version.bind.		0	CH	TXT	"9.2.1"

;; Query time: 208 msec
;; SERVER: 74.54.82.98#53(74.54.82.98)
;; WHEN: Sun Apr  8 19:57:44 2012
;; MSG SIZE  rcvd: 48
```

ვხედავთ რომ BIND 9.2.1-ს იყენებს.

## nslookup

```
root@bt:~$ nslookup
> server sk.s5.ans1.ns98.ztomy.com
Default server: sk.s5.ans1.ns98.ztomy.com
Address: 74.54.82.98#53
> set class=chaos
> set type=txt
> version.bind
Server:		sk.s5.ans1.ns98.ztomy.com
Address:	74.54.82.98#53

version.bind	text = "9.2.1"
```

# ბმულები

ftp://ftp.rs.internic.net
