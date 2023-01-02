IP-შ დაბალი ფენის შემოწმება შეუძლია ხელსაწყოებს როგორც nmap-ს, hping2-ს, და firewalk-ს.
ზოგჯერ სისუსტეები არსებობს რომ იმიტომ რომ ზოგიერთმა TCP სერვისმა იმუშავოს ფაირვოლის მიუხედავათ, ამ სისუსტეები კომბინირება სხვა შეტაკებებთან ერთად შეიძლება.

IP-ს დაბალი ფენის შემოწმებით შეგიძლია დაადგინო შემდეგი ინფორმაცია:

  * რამდენი ხანია რაც სერვერი ჩართულია (TCP დროის ნიმუშის პარამეტრის ანალიზით)
  * TCP სერვისები რომლებიც დაშვებულია ფაირვოლის მიერ (TCP და ICMP პასუხების ანალიზით)
  * TCP თანმიმდევრობა და IP ID-ს მატულობა (increment), ამის დადგენა პროგნოზით შეიძლება
  * სერვერის ოპერაციული სისტემა (IP ანაბეჭდის გამოყენებით)

TCP დროის ნიმუში აღწერილია RFC 1323-ში; მაგრამ ბევრი ოპერაციული სისტემა არ მიჰყვება RFC 1323-ს. ლინუქსი და *BSD კი მიჰყვება.


# TCP სკანირების პასუხების ანალიზი

TCP ანალიზი სრულდება ერთ-ერთი პასუხით ოთხნაირი პასუხის შორის. ამით პოტენციელურად ვადგენთ თუ კავშირი დამყარდა, ან რატომ ვერა (rejected), გაუქმება (drop), ან დაიკარგა.

საპასუხოთ, შემდეგი ტიპის პაკეტი მოვა:
  * TCP SYN/ACK: პორტი ღიაა
  * TCP RST/ACK: გაგზავნილი პაკეტი არ დაუშვა (rejected), სერვერმა ან სხვა ფაირვოლმა
  * ICMP type 3 code 13: ACL-ელის (Acess Control List) მიერ დაუშვებულია ასეთი კავშირი, სერვერის ან ფაირვოლის დონეზე.
  * არანაირი პასუხი: დამცველმა დანადგარმა "ჩუმად" გააუქმა კავშირი.

## hping2

hping2-ი იძლევა საშუალებას რომ ხელოვნურად შექმნა პაკეტები (სხვადასხვა პარამეტრებით) და გაუგზავნო მსხვერპლს ანალიზისთვის. ანალიზის დროს შეგიძლია გაარკვიო როგორი ფილტრაციის წესები მოქმედებს მტრის ქსელში. 

**hping2-ი საკმაოდ რთული ხელსაწყოა და მრავალი პარამეტრი აქვს.**

| პარამეტრი    | აზსნა განმარტება                           |
| -c <number>  | ანალიზისთვის გასაგზავნი პაკეტის რაოდენობა  |
| -s <port>    | წყარო TCP პორტი (ჰაზარტულას ამორჩეული)  |
| -d <port>    | მიმღები TCP პორტი                         |
| -S           | ააქტიურებს TCP SYN დროშას                 |
| -F           | ააქტიურებს TCP FIN დროშას                 |
| -A           | ააქტიურებს TCP ACK დროშას                 |
 

მაგალითში: სამი TCP SYN პაკეტია გაგზავნილი 192.168.0.1 მიმართ, პორტ 139-ზე, გამოყენებული წყარო პორტი 53-ია (ბევრი ფაირვოლი ისეა კონფიგურირებული რომ ტრეფიკს უშვებს თუ წყარო პორტი 53-ია). 

```
# hping2 -c 3 -s 53 -p 139 -S 192.168.0.1

HPING 192.168.0.1 (eth0 192.168.0.1): S set, 40 headers + 0 data

ip=192.168.0.1 ttl=128 id=275 sport=139 flags=SAP seq=0 win=64240

ip=192.168.0.1 ttl=128 id=276 sport=139 flags=SAP seq=1 win=64240

ip=192.168.0.1 ttl=128 id=277 sport=139 flags=SAP seq=2 win=64240
```

მაგალითი: TCP პორტი 80 ღიაა.

```
# hping2 -c 3 -s 53 -p 80 -S google.com

HPING google.com (eth0 216.239.39.99): S set, 40 headers + 0 data

ip=216.239.39.99 ttl=128 id=289 sport=80 flags=SAP seq=0 win=64240

ip=216.239.39.99 ttl=128 id=290 sport=80 flags=SAP seq=1 win=64240

ip=216.239.39.99 ttl=128 id=291 sport=80 flags=SAP seq=2 win=64240
```

მაგალითი: TCP პორტი 139 დახურულია ან პორტთან კავშირი აკრძალურია (rejected) ფაირვოლის მიერ:

```
# hping2 -c 3 -s 53 -p 139 -S 192.168.0.1

HPING 192.168.0.1 (eth0 192.168.0.1): S set, 40 headers + 0 data

ip=192.168.0.1 ttl=128 id=283 sport=139 flags=R seq=0 win=64240

ip=192.168.0.1 ttl=128 id=284 sport=139 flags=R seq=1 win=64240

ip=192.168.0.1 ttl=128 id=285 sport=139 flags=R seq=2 win=64240
```

მაგალითი: TCP პოტრი 23 დაბლოკილია როუტერის ACL-ის მიერ:

```
# hping2 -c 3 -s 53 -p 23 -S gw.example.org

HPING gw (eth0 192.168.0.254): S set, 40 headers + 0 data

ICMP unreachable type 13 from 192.168.0.254

ICMP unreachable type 13 from 192.168.0.254

ICMP unreachable type 13 from 192.168.0.254
```

მაგალითი: TCP პაკეტები, ანალიზისთვის გაგზავნილი, გაუქმებულნი არიან(drop):

```
# hping2 -c 3 -s 53 -p 80 -S 192.168.10.10

HPING 192.168.10.10 (eth0 192.168.10.10): S set, 40 headers + 0 data
```

## firewalk

firewalk-ი არის ხელსაწყო რომელიც რომელიც ახორციელებს ფაირვოლების და პაკეტების ფილტრეაციის დანადგარების ანალიზს, ეს ხდება IP პაკეტების გაგზავნით რომლებსაც TTL-ი ისეთ რაოდენობას წარმოადგენს რომელსაც ვადა გაუვა როუტერის გადალახვის ერთი ნახტომის შემდეგ.

სამი სატუტის მიათითებს იმაზე თუ პაკეტმა ფაირვოლი გასალახა თუ არა:

  * თუ ICMP type 11 code 0 (TTL-ის ციფრმა გადააჭარბა ტრანზიტის დროს) მესიჯი მოვიდა, მაშინ პაკეტმა ფაირვოლი გადალახა, და შემდეგ პასუხი დაისახა.
  * თუ პაკეტი გაუქმებულია (dropped) მიზეზის მითითების გარეშე, ეს დიდი ალბათობით როუტერის დონეზე მოხდა.
  * თუ ICMP type 3 code 13 (კავშირი ადმინისტრატორის მიერ აკრძალურია) მესიჯი მოვიდა, მაშინ მარტივი ფილტრაციის სისტემნა იყენება როგორც მაგალითად როუტერის ACL-ი.

firewalk-ი ეფექტურად მოქმედებს როუტერიან ქსელებში, რაც უარყოფს კომპიუტერებს რომლებიც ფაირვოლით დაცულნი არიან NAT მისამართებით გამოყენებით.
საინტერესო დოკუმენტია firewalk-ის შესახებ: http://dl.packetstormsecurity.net/UNIX/audit/firewalk/firewalk-final.pdf.

Example 4-10 shows firewalk being run against a host to assess filters in 
place for a selection of TCP ports (21, 22, 23, 25, 53, and 80). 
The utility requires two IP addresses: the gateway (gw.test.org in this example) and the target (www.test.org in this example) that is behind the gateway.

firewalk-ით ფილტრების სინჯვა, TCP პორტებზე (21, 22, 23, 25, 53, and 80). საჭიროა ორი IP მისამართი: gateway (მაგალითად gw.test.org) და მსხვერპლი (მაგალითად www.test.org) რომელიც gateway-ს უკანაა.

```
# firewalk -n -S21,22,23,25,53,80 -pTCP gw.test.org www.test.org

Firewalk 5.0 [gateway ACL scanner]

Firewalk state initialization completed successfully.

TCP-based scan.

Ramping phase source port: 53, destination port: 33434

Hotfoot through 217.41.132.201 using 217.41.132.161 as a metric.

Ramping Phase:

 1 (TTL  1): expired [192.168.102.254]
 2 (TTL  2): expired [212.38.177.41]
 3 (TTL  3): expired [217.41.132.201]

Binding host reached.

Scan bound at 4 hops.

Scanning Phase:

port  21: A! open (port listen) [217.41.132.161]
port  22: A! open (port not listen) [217.41.132.161]
port  23: A! open (port listen) [217.41.132.161]
port  25: A! open (port not listen) [217.41.132.161]
port  53: A! open (port not listen) [217.41.132.161]
port  80: A! open (port not listen) [217.41.132.161]

Scan completed successfully.
```

ეს ხელსაწყო მოქმედებას იწყებს traceroute-ით მსხვერპლის კომპიუტერის მიმართ რომ დაადგინოს რამდენი ნახტომია მას და მსხვერპლთან შორის.
შემდეგ იგხვანება ხელოვნურად შექმნილი TCP პაკეტები, სპეციფიკური TTL-ის ვალუტებით.
მსხვერპლისგან მიღებული პაკეტების ანალიზის დროს, უნდა დააკვირდე ICMP type 11 code 0 მესიჯებს, რომ gw.test.org-ს ფილტრაციის უკუღმა ინჟინერია ქნა და დაადგინო პაკეტების ფილტრაციის წესები.


# ICMP პასუხების პასიურად მონიტორინგი

ქსელის და პორტ საკანირების დროს შეგიძლია რომ პასიურად უთვალთვალო თუ რა მიდი-მოდის ქსელში wireshark-ით და tcpdump-ით. ხშირად ნახავთ რომ ICMP პასუხები საზღვარზე ნამყოფ როუტერებისგან და ფაირვოლებიდან მოდიან:

  * ICMP TTL-ის ვალუტა გადააჯარბა ნორმას (type 11 code 0) მესიჯები, გვანიშნებს მარშუტაციის ციკლს
  * ICMP ადმინისტრატორის მიერ აკრძალური (type 3 code 13) მესიჯები, გვანიშნებს რომ ფაირვოლი ან როუტერი ACL-ებით კრძალავს გარკვეული ტიპის პაკეტებს (reject)

ეს ICMP მესიჯები გვარკვევს ქსელის კონფიგურრების შესახებ. აგრეთვე შეიძლება დავადგინოთ IP ალიასები რომლებსაც ფაირვოლი ხმარობს NAT-ისთვის, მაგალითად თუ სინჭავთ საჯარო მისამართს და ხედავთ სნიფერით რომ პასუხი მოდის შიდა კომპიუტერიდან.

# IP-ს ანაბეჭდი

სხვადასხვა ოპერაციული სისტემები სხვადასხვანაირად პასუხბენ ზოგიერთი ტიპის პაკეტებს. პასუხების ანალიზით შესაძლოა რომ დავადგინოთ ოპერაციული სისტემა, ანალიზში შედის შემდეგი პარამეტრები:

  * TCP FIN ტესტირება
  * TCP sequence number (მიმდინარეობის ნიმერი)
  * TCP WINDOW ტესტირება
  * TCP ACK ტესტირება
  * ICMP მესიჯები
  * ICMP ECHO-ს სიზუსტე
  * პასუხი IP ფრაგმენტირების შემთხვევაში
  * IP TOS (type of service) ტესტირება

nmap-ით ანაბეჭდი, ოპერაციული სისტემის დასადგენად

```
# nmap -O -sS 192.168.0.65

Starting nmap 3.45 ( www.insecure.org/nmap/ )

Interesting ports on 192.168.0.65:

(The 1585 ports scanned but not shown below are in state: closed)

Port       State       Service

22/tcp     open        ssh
25/tcp     open        smtp
53/tcp     open        domain
80/tcp     open        http
88/tcp     open        kerberos-sec
110/tcp    open        pop-3
135/tcp    open        loc-srv
139/tcp    open        netbios-ssn
143/tcp    open        imap2
389/tcp    open        ldap
445/tcp    open        microsoft-ds
464/tcp    open        kpasswd5
593/tcp    open        http-rpc-epmap
636/tcp    open        ldapssl
1026/tcp   open        LSA-or-nterm
1029/tcp   open        ms-lsa
1352/tcp   open        lotusnotes
3268/tcp   open        globalcatLDAP
3269/tcp   open        globalcatLDAPssl
3372/tcp   open        msdtc

Remote OS guesses: Windows 2000 or WinXP

Nmap run completed -- 1 IP address (1 host up)
```

## TCP Sequence და IP ID Incrementation

თუ მიზანში ამოღებულ სერვერში TCP მიმდინევრობითი ნომრები (sequence numbers) შექმნილი არიან ისე რომ მათი გამოცნობა ადვილი იყოს მაშინ შესაძლოა ბრმა სპუფირება (blind spoofing) და hijacking-ი (ეს უფრო ქსელის შიდა სივრცეში ხდება).

თუ IP-ს ID ერთ-ერთის მატულობს, მაშინ მიზანში ამოღებული კომპიუტერი შესაძლოა გამოყენდეს როგორც მესამე პირი IP ID header სკანირებისთვის. 
IP ID header სკანირებისთვის საჭიროა რომ, მესამე პირისგან მოსულ პაკეტებში ID მატულობდეს მხოლოდ ერთით, ასე სკანირების ზუსტ რეზულტატს დავადგენთ.

**nmap-ით TCP და IP ID sequence-ების ტესტირება. ხმაურიან რეჟიმში (-v), TCP/IP ანაბეჭდი (-O).**

```
# nmap -v -sS -O 192.168.102.251

Starting nmap 3.45 ( www.insecure.org/nmap/ )

Interesting ports on cartman (192.168.102.251):

(The 1524 ports scanned but not shown below are in state: closed)

Port       State       Service

25/tcp     open        smtp
53/tcp     open        domain
8080/tcp   open        http-proxy


Remote OS guesses: Windows 2000 RC1 through final release

TCP Sequence Prediction: Class=random positive increments

                         Difficulty=15269 (Worthy challenge)

IPID Sequence Generation: Incremental

Nmap run completed -- 1 IP address (1 host up) scanned in 1 second
```
