# თეორია

სნიფერის მეშვეობით შეგიძლია უთვალთვალი ქსელში მონაცემების მიმოქცევას. ანუ ხედავ რა შედის და გადის ლოკალურ  ქსელში. სნიფერებს ქსელის ინჟინერებიც ხმარობენ ქსელური პრობლემების გასაანალიზებლად და ჰაკერებიც.
თუ ქსელში მოძრავი მონაცემები არ არიან შიფრილებული და ქსელის hub-ის გამოყენებული ყოველივე კომპიუტერის შესაერთებლად მაშინ მარტივია ქსელური ტრეფიკის ხელში ჩაგდება: მომხარებლის სახელი, პაროლო, წერილის ტექსტი.
თუ ქსელი swich-ს ხმარობს საქმე უფრო რთულია მაგრამ არა შეუძლებელი. 

რის ნახვა შეიძლება სნიფერით?

არა გაშიფრული მონაცემები:
  * მომხმარებლების მეტსახელები (logins)
  * პაროლები, პროტოკოლებს გააჩნია
  * კავშირის თვალთვალი
  * ელექტრონული წერილების თვალთვალი
  * IP  მისამართების ნახვა
  * MAC მისამართების ნახვა
  * როუტერების IP  მისამართების ნახვ

TODO

# პრაქტიკა

## dsniff

| პლატფორმა | იუნიქსი |
| დაცვა     | შიფრაცია, სნიფერის აღმოსაჩენი პროგრამა |

dsniff-ს შეუძლია მოიპოვოს სხვადასხვა სერვისის პაროლი ქსელში მონაცემების მიმოქცევის ხელში ჩაგდებით და თვალყურით.
მას შემდგომი სერვისების მხარდაჭერა აქვს: FTP, Telnet, SMTP, HTTP, POP, poppass, NNTP, IMAP, SNMP, LDAP, Rlogin, RIP, OSPF, PPTP MS-CHAP, NFS, VRRP, YP/NIS, SOCKS, X11, CVS, IRC, AIM, ICQ, Napster, PostgreSQL, Meeting Maker, Citrix ICA, Symantec pcAnywhere, NAI Sniffer, Microsoft SMB, Oracle SQL*Net, Sybase, და Microsoft SQL.

მაგალითი: ქსელის eth0 ინტერფეისის თვალთვალი, სერვისის ავტომატურად დადგენით (-m)
```
# dsniff -i eth0 -m
```

ეხლა ლოკალურ ქსელში სხვა კომპიუტერით რომ, მაგალითად, FTP სერვერს დაუკავირდე და პაროლი ჩაწერო კავშირის დამყარების დროს, გაშვებული dsniff-ი პაროლს ეკრანზე დაბეჭდავს.

```
dsniff: listening on eth0
-----------------
21/07/19 10:50:07 tcp 192.168.2.30.36761 -> 192.168.2.32.21 (ftp)
USER jamesbond
PASS 007secret
```

## ngrep

| პლატფორმა   | ვინდოუსი ან იუნიქსი |
| წინა პირობა | NULL სესია |
| დაცვა       | შიფრაცია, სნიფერის აღმოსაჩენი პროგრამა |

ngrep არის სნიფერი რომელიც ცნობს IP-ს (Internet Protocol), TCP-ს (Transport Control Protocol), UDP-ს (User Datagram Protocol), ICMP-ს (Internet Control Messenger Protocol), IGMP-ს (Internet Group Messenger Protocol), PPP- (Point to Point Protocol), SLIP-ს (Serial Line Interface Protocol), FDDI, Token Ring, Null ინტერფეისები. თან შეუძლია მუშაობა BPF-ით (Berkley Packet Filter).

ngrep გამოსათიშად დააჭირეთ CTRL-ს და C-ს.

**სინტაქსი:**

```
ngrep <options>
```

**მაგალითი 1: უბრალო ngrep-ის გაშვება**

```
ngrep
```

**მაგალითი 2: ngrep-ის გაშვება და რეზულტატების ფაილში ჩაწერა**

```
ngrep >> rezultatebi.txt
```

## tcpdump

| პლატფორმა   | იუნიქსი |
| წინა პირობა | NULL სესია |
| დაცვა       | შიფრაცია, სნიფერის აღმოსაჩენი პროგრამა |

tcpdump-ი ხელში იგდებს პაკეტებს რომლებიც ლოკალურ ქსელში შედის და გადის.
tcpdump-ი არის ერთერთი, ძლიერი და ყველაზე ცნომილი და პროფესიონალური სნიფერი.

**სინტაქსი:**

```
tcpdump <options>
```

**მაგალითი 1: ყოველივე პაკეტის ხელში ჩაგდება**

```
tcpdump
```

**მაგალითი 2: ფილტრის გამოყენება ICMP პაკეტების ხელში ჩასაგდებად როდესც 192.168.1.38-ი 192.168.1.48-ს უგზავნის ICMP პაკეტს, მაგალითად ping-ს**

```
# sudo tcpdump -n -t -X -i wlan0 -s 64 icmp and src 192.168.1.38 and dst 192.168.1.48
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on wlan0, link-type EN10MB (Ethernet), capture size 64 bytes
IP 192.168.1.38 > 192.168.1.48: ICMP echo request, id 10618, seq 1, length 64
	0x0000:  4500 0054 25f8 4000 4001 910a c0a8 0126  E..T%.@.@......&
	0x0010:  c0a8 0130 0800 99d1 297a 0001 7e0f 2d5d  ...0....)z..~.-]
	0x0020:  0000 0000 c073 0a00 0000 0000 1011 1213  .....s..........
	0x0030:  1415                                     ..
IP 192.168.1.38 > 192.168.1.48: ICMP echo request, id 10618, seq 2, length 64
	0x0000:  4500 0054 261a 4000 4001 90e8 c0a8 0126  E..T&.@.@......&
	0x0010:  c0a8 0130 0800 c6d1 297a 0002 7f0f 2d5d  ...0....)z....-]
	0x0020:  0000 0000 9272 0a00 0000 0000 1011 1213  .....r..........
	0x0030:  1415
```

პარამეტრების ახსნა:
  * -n: არ გადაიყვანო მისამართი სახელწოდებად.
  * -t: არ დაბეჭდო timestamp-ი
  * -X: პაკეტის headers და მონაცემები დაბეჭდე hex და ASCII-ში
  * -i wlan0:სნიფინგი ხდება wlan0 ინტერფეისზე
  * -s 64: ყოველივე პაკეტის ყოველი მონაცემის ზომა იყოს 64 ბაიტი
  * icmp and src 192.168.1.38 and dst 192.168.1.48: ხელში ვიგდებთ ICMP ტრეფიკს როცა 192.168.1.38-ი  192.168.1.48-ს უგზავნის ICMP პაკეტს.

**მაგალითი 3: ლოგ ფაილში ჩაწერა**

```
tcpdump -w ფაილის_სახელი
```

**მაგალითი 4: ლოგ ფაილის წაკითხვა**

```
tcpdump -r ფაილის_სახელი
```

**ფილტრების დანიშნულება**

| დანიშნულება | ფილტრი |
| სერვერები და ქსელები | host, net |
| ნაკადის მიმართულება | dst, src |
| პორტები | port, portrange |
| Ethernet-თან ერთად მომუშავე პროტოკოლები | ether proto ip, ip6,arp, rarp, atalk, aarp, decnet, ipx… |
| IP-სთან ერთად მომუშავე პროტოკოლები | ip proto icmp, udp, tcp |

and-ით და or-ით ამ ყველაფრის კომბინირება შეიძლება.

# IPDump2

| პლატფორმა | ვინდოუსო ან იუნიქსი |
| წინა პირობა | არაფერი |
| დაცვა 	    | შიფრაცია, სნიფერის აღმოსაჩენი პროგრამა |

IPDump2-ი გამოიყენება ქსელის მონიტორინგისთვის, მოპოვებული ინფორმაციის ფაილში შენახვა შესაძლოა. 
ამ პროგრამას ვიყენებთ swith bypassing ტექნიკებთან ერთად, რომ დავადგინოთ რომელი კომპიუტერები რომელ სერვერთან კავშირობენ.
IPDump2 პაკეტების დეტალებს არ აჩვენებს. იგი მარტო აჩვენებს რომელი კომპიუტერი რომელ სხვა კომპიუტერს უკავშირდება და რომელ პორტზე.

IPDump2 რომ გააჩეროთ დააჭირეთ CTRL-ს და C-ს.

**სინტაქსი:**

```
IPDump2 ინტერფეისი <options>
```

**მაგალითი 1: eth0 ინტერფეისზე მუშაობა**

```
ipdump2 eth0
```

ეკრანზე გამიჩდება პაკერების source IP და პორტი და ასევე destination IP და პორტი.
უფრო სწორად, აჩვენებს ასე:

დღე - დრო - პროტოკოლი - წყარო IP წყარო პორტი - destination IP destination პორტი

**მაგალითი 2: eth0 ინტერფეისზე მუშაობა და რეზულტატების ფაილში შენახვა**

```
ipdump2 eth0 >> rezultatebi.txt
```

Note:
ვინდოუსში ინტერფეისის სახელი შეიძლება იყოს 0.

## Sniffit

| პლატფორმა   | იუნიქსი |
| წინა პირობა | არაფერი |
| დაცვა       | Access Control Lists (ACLs), bastion servers/workstations, host-base firewalls |

Sniffit-ს შეუძლია ხელში ჩაიგდოს TCP, UDP, ICMP პაკეტები, ეთერნეტის და PPP მოწყობილობების დეტექტირება, ფილტრის შექმნა, და რეზულტატების ფაილში შენახვა.

Sniffit-ი რომ გააჩეროთ დააჭირეთ CTRL-ს და C-ს.

**სინტაქსი:**

```
sniffit <options>
```

**მაგალითი 1:**

```
sniffit -s 176.53.66.38  -x -a -F eth0
```

-s 176.53.66.38 უბრძანებს რომ ეს IP მისამართი იყოს გამოყენებული როგორც წყარო IP
-x უბრძანებს რომ პაკეტების დამატებითი ინფორმაციაც აჩვენოს
-a კარგად ახსნილი არ არის
-F eth0 უბრძანებს რომ eth0 ინტერფეისი იხმაროს

**მაგალითი 2: შეგიძლიათ რომ რეზულტატები ფაილში შეინახოთ**

```
sniffit -s 176.53.66.38  -x -a -F eth0 >> rezultatebi.txt
```

## Wireshark

| პლატფორმა   | ვინდოუსი და იუნიქსი |
| წინა პირობა | უნდა გქონდეს root-ის უფლებები |
| დაცვა       | შიფრაცია, სნიფერის აღმოსაჩენი პროგრამა |

გაუშვი Wireshark-ი:

```
# sudo wireshark
```

სნიფინგის დასაწყებად შეარჩიე ქსელის ინტერფეისი რომელზედ სნიფინგი გაიმართება და შემდეგ დაუწკაპუნე ლურჯ ღილაკს (Start capturing packets).

![Wireshark start](https://raw.githubusercontent.com/zonkey21/zonkey21.github.io/master/pictures/wireshark_start.png "Wireshark start")

თუ ქსელში მიმოსვლაა პაკეტები დაიბეჭდება.

![Wireshark capturing](https://raw.githubusercontent.com/zonkey21/zonkey21.github.io/master/pictures/wireshark_capturing.png "Wireshark capturing")

თუ გინდა რომ ჩვენებული იყოს მხოლოდ გარკვეული ტიპის პაკეტები შეგიძლია იხმარო ფილტრი. მაგალითად მხოლოდ ICMP ტიპის პაკეტების სანახავად ჩაწერ icmpv6-ს.

![Wireshark icmp](https://raw.githubusercontent.com/zonkey21/zonkey21.github.io/master/pictures/wireshark_icmp.png "Wireshark icmp")

თუ გინდა რომ სნიფინგი შეჩერდეს უნდა დაუწკაპუნო წითელ ღილაკს (Stop capturing packets).

![Wireshark stop](https://raw.githubusercontent.com/zonkey21/zonkey21.github.io/master/pictures/wireshark_stop.png "Wireshark stop")

სნიფინგის პარამეტრების შესარჩევად უნდა დაუწკაპუნო Capture და შემდეგ Options.

![Wireshark options](https://raw.githubusercontent.com/zonkey21/zonkey21.github.io/master/pictures/wireshark_options.png "Wireshark options")
![Wireshark optionsi input](https://raw.githubusercontent.com/zonkey21/zonkey21.github.io/master/pictures/wireshark_options_input.png "Wireshark options input")
![Wireshark optionsi options](https://raw.githubusercontent.com/zonkey21/zonkey21.github.io/master/pictures/wireshark_options_options.png "Wireshark options options")


## სხვა სნიფერები

  - **WinDump** (tcpdump-ის ვინდოუსის ვერცია)
  - **ZxSniffer** (ვინდოუსისთვის, იჭერს პაროლებს შემდეგი პროტოკოლებისთვის: POP3, FTP, ICQ, HTTP)
