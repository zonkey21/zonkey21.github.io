აქ განვიხილავთ თუ როგორ უნდა აღმოაჩინო აპარატები მიზნად აღებულ ქსელში.
გარკველი ქსელის შესახებ ინფორმაციების დაგროვების შემდეგ, საძიებო სისტემების მეშვეობოთ და სხვა, დროა რომ არმოვაჩინოთ მიზანში აღებულ ქსელში შეერთებული აპარატები, და დავადგინოთ თუ შეგვიძლია ისინი მსხვერპლად ვაქციოთ. პროცესი არის შემდგომი:
  * ქსელში არსებული აპარატების აღმოჩენა შეტაკების განსახორციებლად.
  * არსებული აპარატის ოპერაციული სისტემის დადგენა.

წინად დაგროვებული ინფორმაცია დაგვეხმარება მოწყვლადობის შეფასებაში.

# სამიზნე კომპიუტერის არმოჩენა

##  ping

ping-ი განთქმული ხელსაწყოა ქსელში არსებული კომპიუტერის აღმოსაჩენად. იგი მოქმედებს ICMP echo request-ის გაგზავნით სატესტო კომპიუტერთან. თუ ის კომპიუტერი ქსელში არსებულია და ფაირვოლი არ აბკოლებს ICMP echo request-ს მაშინ დამიზნური კომპიუტერი უპასუხებს ICMP echo reply პაკეტით.

```
tutashxia@kali ~> ping hackthissite.org
PING hackthissite.org(hackthissite.org (2001:41d0:8:ccd8:137:74:187:101)) 56 data bytes
64 bytes from hackthissite.org (2001:41d0:8:ccd8:137:74:187:101): icmp_seq=1 ttl=57 time=7.92 ms
64 bytes from hackthissite.org (2001:41d0:8:ccd8:137:74:187:101): icmp_seq=2 ttl=57 time=8.21 ms
64 bytes from hackthissite.org (2001:41d0:8:ccd8:137:74:187:101): icmp_seq=3 ttl=57 time=8.21 ms
64 bytes from hackthissite.org (2001:41d0:8:ccd8:137:74:187:101): icmp_seq=4 ttl=57 time=7.63 ms
64 bytes from hackthissite.org (2001:41d0:8:ccd8:137:74:187:101): icmp_seq=5 ttl=57 time=7.66 ms
64 bytes from hackthissite.org (2001:41d0:8:ccd8:137:74:187:101): icmp_seq=6 ttl=57 time=10.6 ms
^C
--- hackthissite.org ping statistics ---
6 packets transmitted, 6 received, 0% packet loss, time 5008ms
rtt min/avg/max/mdev = 7.634/8.379/10.624/1.032 ms
```

ping-ი რომ შეაჩეროთ: CTRL-C

ping-ის საინტერესო option-ები:
  * -c (count): გასაგზავნი ICMP echo request-ების რაოდენობა.
  * -I (Interface): ქსელის ინტერფეისი (მაგალითად eth0) რომლიდანაც გაიგზავნება პაკეტები.
  * -s (size): პაკეთში მოთავსებული მონაცემების რაოდენობა ბაიტებში. დეფულტია 56 რომელიც 64 ხდება თუ ICMP header-ის 8 ბაიტს დავუმატებთ.

### დაცვა

ping-ის დასაბლოკად, ფაირვოლის მეშვეობით echo request-ის პაკეტების შემოსვლას ნება მიეცით მხოლოდ ნაცნობი მისამართებიდან და სხვა მისამართებიდან ასეთ პაკეტებს არ უპასუხოთ (drop).

## arping

arping-ი არის ლოკალულ ქსელში (Local Area Network, LAN) პინგის გასაგზავნად ARP (Address Resolution Protocol) request-ის მეშვეობით. arping-ს უნდა მიუნიშნოთ სამიზნის IP მისამართი, host-ის სახელი MAC მისამართი (Media Access Control). arping-ი OSI-ის მეორე ფენაში მოქმედებს და მხოლოდ ლოკალურ ქსელში შეუძლია მოქმედება, მისი მარშუტაცია (routing) შეუძლებალია.

```
root@kali:~# arping 192.168.1.254 -c 1
ARPING 192.168.1.254
42 bytes from a2:8a:12:00:e6:6e (192.168.1.254): index=0 time=1.894 msec

--- 192.168.1.254 statistics ---
1 packets transmitted, 1 packets received,   0% unanswered (0 extra)
rtt min/avg/max/std-dev = 1.894/1.894/1.894/0.000 ms
```

ვხედავთ რომ მსხვერპლის MAC-ია a2:8a:12:00:e6:6e.

### მისამართის გამოყენების სტატუსის დადგენა

თუ ქსურთ რომ დაადგინოთ გარკვეული IP მისამართი უკვე იყო გამოყენებული თუ არა ლოკალურ ქსელში ქენით ასე:

```
root@kali:~# arping -d -i wlan0 192.168.1.254 -c 2
root@kali:~# echo $?
root@kali:~# 1
```

თუ გიჩვენებთ 1-ს, ეს ნიშნავს რომ ეს მისამართი უკვე იყო რამოდენიმეჯერ გამოყენებული. თუ 0-ია ესეიგი ეს მისამართი თავისუფალია.

## fping

ping-ისგან განსხვავებით fping-ი ბევრ მისამართს ტესტავს ერთდროულად, იგი 3 ICMP echo request-ს აგზავნის.

```
tutashxia@kali ~> fping www.apple.com www.microsoft.com www.adobe.com
www.apple.com is alive
www.microsoft.com is alive
www.adobe.com is alive
```

მას აგრეთვე შეუძლია გარკევეული ქსელის ყოველივე მისამართი შეამოწმოს, ნისამართების სიის შექმნის მეშვეობით:

```
tutashxia@kali ~> fping -g 180.45.32.0/24
180.45.32.8 is alive
180.45.32.19 is alive
180.45.32.28 is alive
...
180.45.32.253 is unreachable
180.45.32.254 is unreachable
```

მას აგრეთვე შეუძლია სტატისტიკის ჩვენება:

```
tutashxia@kali ~> fping -s www.apple.com www.microsoft.com www.adobe.com
www.apple.com is alive
www.microsoft.com is alive
www.adobe.com is alive

       3 targets
       3 alive
       0 unreachable
       0 unknown addresses

       0 timeouts (waiting for response)
       3 ICMP Echos sent
       3 ICMP Echo Replies received
       0 other ICMP received

 3.24 ms (min round trip time)
 3.28 ms (avg round trip time)
 3.31 ms (max round trip time)
        0.024 sec (elapsed real time)
```

##hping3

hping3-ის შეუძლია პაკეტების შექმნა და ქსელის ანალიზი.
რახან მას შეუძლია პაკეტების ხელოვნურად შექმნა, უპირატესობა ეძლევა TCP/IP-ის უსაფრთხოების შემოწმებისთვის ხერხებით როგორიცაა პორტების სკანირება, ფაირვოლის წესების შემოწმება, IDS-ის შემოწმება (Intrusion Detection System), TCP/IP-ის წნობილი ნაკლების ექსპლუატაცია და ქსელის სისწრაფის დადგენა.

hping3-ის გამოყენება შესაძლოა 3 ნაირად: command line-ი, ინტერაქტიული shell-ი ან სკრიპტი.

hping3-ს თუ გაუშვებთ პარამეტრის გარეშე იგი მხოლოდ null TCP პაკეტს გააგზავნის პორტ 0-ზე.

| მოკლე პარამეტრი | გრძლი პარამეტრი | განმარტება             |
| -0              | --raw-ip        | აგზავნის raw IP პაკეტს | 
| -1              | --icmp          | აგზავნის ICMP პაკეტს   |
| -2              | --udp           | აგზავნის UDP პაკეტს    |
| -8              | --scan          | სკანირების რეჟიმი      |
| -9              | --listen        | სმენის რეჟიმი          | 


აგრეთვე შეგვიძლია მივუთითოთ TCP დროშები:

| პარამეტრი | დროშის სახელწოდება | 
| -S              | syn          |
| -A              | ack          |
| -R              | rst          |
| -F              | fin          |
| -P              | psh          |
| -U              | urg          |
| -X              | xmas: flags fin, urg, psh set        |
| -Y              | ymas         |

გავაგზავნოთ ICMP echo request-ი, ჰოსტია , -1 (პროტოკოლისთვის) და -c 1 (counter, 1 პაკეტი).

```
root@kali:~# hping3 -1 192.168.1.254 -c 1
HPING 192.168.1.254 (wlan0 192.168.1.254): icmp mode set, 28 headers + 0 data bytes
len=28 ip=192.168.1.254 ttl=64 id=30364 icmp_seq=0 rtt=7.7 ms

--- 192.168.1.254 hping statistic ---
1 packets transmitted, 1 packets received, 0% packet loss
round-trip min/avg/max = 7.7/7.7/7.7 ms
```

აგრეთვე შეგიძლიათ hping3-ი იხმაროთ ინტერაქტიულად, TCL ენის მეშვეობით. მაგალითი:

```
root@kali:~# hping3 
hping3> hping send {ip(daddr=192.168.1.254)+icmp(type=8,code=0)}
```

რომ სერვერისგან პასუხი მიიღოთ: 

```
hping3> hping recv wlan0
ip(ihl=0x5,ver=0x4,tos=0x00,totlen=784,id=7357,fragoff=0,mf=0,df=1,rf=0,ttl=56,proto=6,cksum=0x6b8c,saddr=191.201.21.130,daddr=192.168.1.38)+tcp(sport=443,dport=32836,seq=2382152575,ack=3220138325,x2=0x0,off=8,flags=pa,win=342,cksum=0xd6a5,urp=0)+tcp.nop()+tcp.nop()+tcp.timestamp(val=3810015318,ecr=3270811978)+data(str=<მონაცემები>)
```

აგერთვე შეგიძლიათ გამოიყენოთ hping3-ი ფაირვოლის წესების შემოწმებისთვის. ვთქვათ დააწესეთ ესეთი წესები:
  * დაშვებული 22-ე (ssh) პორტზე დაკავშირება TCP პროტოკოლით.
  * დაშვენილია ყოველივე TCP კავშირი თუ ქსელური მიმოსვლა უკვე არსებულია.
  * სხვა პაკეტები იკრძალება.

ამ წესების შემოწმება ხდება ასე:

```
root@kali:~# hping3 -1 192.168.1.254 -c 1
```

თუ საპასუხო პაკეტი არ მოვიდა ეს ნიშნავს რომ 3-მე წესუ დაცულია.

22-ე პორტზე გავაგზავნოთ პაკეტი აქტივებული SYN დროშით.

```
root@kali:~# hping3 192.168.1.254 -c 1 -S -p 22 -s 6060
```

თუ პაკეტი არ დაიკარგა ესეიგი პირველო წესი დაცულია.

ვნახოთ თუ 22-ე პორტზე UDP პაკეტის გაგზავნას შევძლებთ:

```
root@kali:~# hping3 -2 192.168.1.254 -c 1 -S -p 22 -s 6060
```

თუ წესი დაცულია წესით ICMP port unreachable უნდა დაიწეროს.

## nping

nping-ით შესაძლოა სხვადასხვა ქსელური პროტოკოლის პაკეტის შექმნა (TCP, UDP, ICMP და ARP). დამატებით შეგიძლიათ header field-ები შეავსოთ ისე როგორც გინდათ, მაგალითად გააყალბოთ წყარო მისამართი. nping-ს აგრეთვე შეუძლია რომ ინუშაოს მრავალ მისამართებზე და პორტებზე. ამ ხელსაწყოთი შეგიძლიათ ქსელის დატვირთვის ტესტირება, ARP მოწამვლა (ARP poisoning), და სერვისის დატვირთვა (DoS შეტაკება).

| რეჟიმი        | აღწერა                       |
| --tcp-connect | არა პრივილეგიური TCP კავშირი |
| --tcp         | TCP რეჟიმი |
| --udp         | UDP რეჟიმი |
| --icmp        | ICMP რეჟიმი (თავისთავად ამ რეჟიმშია) |
| --arp         | ARP/RARP რეჟიმი                      |
| --tr          | traceroute-ის რეჯჟიმი (იყენება მხოლოდ TCP/UDP/ICMP რეჟიმებში) |


ICMP echo request-ის გაგზავნა გარკვეული IP მწკრივისთვის:

```
tutashxia@kali ~> nping -c 1 172.217.21.174-182

Starting Nping 0.7.70 ( https://nmap.org/nping ) at 2019-01-26 19:21 CET
SENT (0.0015s) Starting TCP Handshake > 172.217.21.174:80
RCVD (0.0333s) Handshake with 172.217.21.174:80 completed
SENT (1.0037s) Starting TCP Handshake > 172.217.21.175:80
RCVD (1.0360s) Handshake with 172.217.21.175:80 completed
SENT (2.0063s) Starting TCP Handshake > 172.217.21.176:80
RCVD (2.0386s) Handshake with 172.217.21.176:80 completed
SENT (3.0090s) Starting TCP Handshake > 172.217.21.177:80
RCVD (3.0424s) Handshake with 172.217.21.177:80 completed
SENT (4.0118s) Starting TCP Handshake > 172.217.21.178:80
RCVD (4.0433s) Handshake with 172.217.21.178:80 completed
SENT (5.0142s) Starting TCP Handshake > 172.217.21.179:80
RCVD (5.0465s) Handshake with 172.217.21.179:80 completed
SENT (6.0169s) Starting TCP Handshake > 172.217.21.180:80
RCVD (6.0485s) Handshake with 172.217.21.180:80 completed
SENT (7.0196s) Starting TCP Handshake > 172.217.21.181:80
RCVD (7.0522s) Handshake with 172.217.21.181:80 completed
SENT (8.0225s) Starting TCP Handshake > 172.217.21.182:80
RCVD (8.0560s) Handshake with 172.217.21.182:80 completed
 
Statistics for host 172.217.21.174:
 |  Probes Sent: 1 | Rcvd: 1 | Lost: 0  (0.00%)
 |_ Max rtt: 31.903ms | Min rtt: 31.903ms | Avg rtt: 31.903ms
Statistics for host 172.217.21.175:
 |  Probes Sent: 1 | Rcvd: 1 | Lost: 0  (0.00%)
 |_ Max rtt: 32.304ms | Min rtt: 32.304ms | Avg rtt: 32.304ms
Statistics for host 172.217.21.176:
 |  Probes Sent: 1 | Rcvd: 1 | Lost: 0  (0.00%)
 |_ Max rtt: 32.300ms | Min rtt: 32.300ms | Avg rtt: 32.300ms
Statistics for host 172.217.21.177:
 |  Probes Sent: 1 | Rcvd: 1 | Lost: 0  (0.00%)
 |_ Max rtt: 33.519ms | Min rtt: 33.519ms | Avg rtt: 33.519ms
Statistics for host 172.217.21.178:
 |  Probes Sent: 1 | Rcvd: 1 | Lost: 0  (0.00%)
 |_ Max rtt: 31.621ms | Min rtt: 31.621ms | Avg rtt: 31.621ms
Statistics for host 172.217.21.179:
 |  Probes Sent: 1 | Rcvd: 1 | Lost: 0  (0.00%)
 |_ Max rtt: 32.394ms | Min rtt: 32.394ms | Avg rtt: 32.394ms
Statistics for host 172.217.21.180:
 |  Probes Sent: 1 | Rcvd: 1 | Lost: 0  (0.00%)
 |_ Max rtt: 31.617ms | Min rtt: 31.617ms | Avg rtt: 31.617ms
Statistics for host 172.217.21.181:
 |  Probes Sent: 1 | Rcvd: 1 | Lost: 0  (0.00%)
 |_ Max rtt: 32.596ms | Min rtt: 32.596ms | Avg rtt: 32.596ms
Statistics for host 172.217.21.182:
 |  Probes Sent: 1 | Rcvd: 1 | Lost: 0  (0.00%)
 |_ Max rtt: 33.528ms | Min rtt: 33.528ms | Avg rtt: 33.528ms
TCP connection attempts: 9 | Successful connections: 9 | Failed: 0 (0.00%)
Nping done: 9 IP addresses pinged in 8.06 seconds
```

აქ ვხედავთ რომ ყოველივე ჰოსტმა გვიპასუხა. ერთერთ ჰოსტს ICMP echo request-ისთვის რომ არ ეპასუხა, შეგვეძლო გაგვეგზავნა TCP SYN პაკეტეი რომ დაგვედგინა ის ჰოსტი აქტიურია თუ არა:

TCP რეჟიმში ვაგზავნით 1 პაკეტს (-c 1), 80-ე პორტზე (-p).

```

root@kali:~# nping --tcp -c 1 -p 80 172.217.21.174

Starting Nping 0.7.70 ( https://nmap.org/nping ) at 2019-01-26 19:27 CET
SENT (0.1883s) TCP 192.168.1.38:48515 > 172.217.21.174:80 S ttl=64 id=46256 iplen=40  seq=3566650121 win=1480 
RCVD (0.2218s) TCP 172.217.21.174:80 > 192.168.1.38:48515 SA ttl=111 id=37932 iplen=44  seq=2136301839 win=60720 <mss 1380>
 
Max rtt: 33.394ms | Min rtt: 33.394ms | Avg rtt: 33.394ms
Raw packets sent: 1 (40B) | Rcvd: 1 (44B) | Lost: 0 (0.00%)
Nping done: 1 IP address pinged in 1.22 seconds
```

რახან გვიპასუხა ვხედავთ რომ ეს ჰოსტი აქტიურია.

ხშირად ღია პორტებია: 21, 22, 23, 25, 80, 443, 8080, 8443.

## alive6

თუ გინდა რომ IPv6 სივცეში აღმოაჩინო არსებული მისამართები მთლიანად ლოკალური ქსელის სკანირება არაა სასურველი რახან ის დიდია, ქსელის პრეფიქსი კოდირებულია 64-ი ბიტით.

ლოკალურ ქსელში მისამართების აღმოსაჩენად არსებობს ICMPv6 Neighbour Discovery პროტოკოლი რომელიც link-local და auto-configured მისამართებს პოულობს ლოკალურ ქსელში.

```
root@kali:~# atk6-alive6 -p wlan0
Alive: 4b35:d54:bd0c:cb40::1 [ICMP echo-reply]

Scanned 1 address and found 1 system alive
```

**დაკვირვება**: wlan0 არის ქსელის ინტერფეისის სახელწოდება.

### დაცვა

თუ არ გსურთ რომ სხვამ აღმოაჩინოს თქვენი კომპიუტერის მისამართი შეგიძლიათ რონ ფაირვოლის მეშვეობით ამის საშუალება დააბრკოლოდ:

```
root@kali:~# ip6tables –A INPUT –p ipv6-icmp –-type icmpv6-type 128 –j DROP
```

## detect-new-ip6

ამ ხელსაწყოს მეშვეობით გაიგებთ თუ ახალი IPv6 მისამართი შეურთდა თქვენს ლოკალურ ქსელს.

```
tutashxia@kali ~> atk6-detect-new-ip6 wlan0
```

**დაკვირვება**: wlan0 არის ქსელის ინტერფეისის სახელწოდება.

## passive_discovery6

თუ გსურთ რომ პასიურად აღმოაჩინოთ ახალი IPv6 მისამართები თქვენს ლოკალურ ქსელში შეგიძლიათ იხმაროთ ეს sniffer-ი. ამის გამოყენებით თან თავს ააცლით IDS-ს.

```
tutashxia@kali ~> atk6-passive_discovery6 wlan0
```

**დაკვირვება**: wlan0 არის ქსელის ინტერფეისის სახელწოდება.

## nbtscan

nbtscan-ის გამოყენება სახარბელია ქსელში რომელშიც ვინდოუსებია შეერთებული. იგი აჩვენევს NetBIOS-ის შესახებ IP მისამართს, NetBIOS-ის მომხმარებლის სახელს, აქტიურ სერვისს, ჰოსტის MAC მისამართს.
ეს ხელსაწყო დიდ ტრეფიკს ქმნის და შანსია რომ ქსელის ადმინისტრატორის ლოგებში დაფიქსირდეს.

ლოკალური ქსელის შემოწმება 192.168.1.0/24:

```
tutashxia@kali ~> nbtscan 192.168.1.1-254
Doing NBT name scan for addresses from 192.168.1.1-254

IP address       NetBIOS Name     Server    User             MAC address      
------------------------------------------------------------------------------
192.168.1.254    ROUTER          <server>  ROUTER          00:00:00:00:00:00
```

NetBIOS-ის სერვისების დადგენა:
-h (human) ანიშნებს რომ ადანიანისთვის გასაგები პასუხი დაბეჭდოს, -v (verbose) განმარტებით.

```
tutashxia@kali ~> nbtscan -hv 192.168.1.1-254
Doing NBT name scan for addresses from 192.168.1.1-254


NetBIOS Name Table for Host 192.168.1.254:

Incomplete packet, 317 bytes long.
Name             Service          Type             
----------------------------------------
ROUTER          Workstation Service
ROUTER          Messenger Service
ROUTER          File Server Service
ROUTER          Workstation Service
ROUTER          Messenger Service
ROUTER          File Server Service
__MSBROWSE__  Master Browser
WORKGROUP        Master Browser
WORKGROUP        Browser Service Elections
WORKGROUP        Domain Name
WORKGROUP        Master Browser
WORKGROUP        Browser Service Elections

Adapter address: 00:00:00:00:00:00
----------------------------------------
```

ვხედავთ რომ არის სერვისი File Server, შემდგომ შეგვიძლია რომ ვნახოთ ეს სერვისი ღიაა თუ არა რომ ფაილები მოვიპოვოთ.
