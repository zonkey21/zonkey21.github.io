# თეორია

TODO

# პრაქტიკა

## Packit

### პაკეტების ხელში ჩაგდება

| პლატფორმა   | იუნიქსი |
| წინა პირობა | არაფერი |
| დაცვა       | Firewall filters, vendor pacthes where applicable |

Packit-ს შეუძლია გააყალბოს IP და MAC მისამართი, customize, inject, monitor, manipulate IP traffic.
შემდეგი პროტოკოლების გაყალბება შეუძლია: TCP, UDP, ICMP, IP, ARP, RARP, Ethernet header options.
Packit-ი კარგია firewall, IDS (Intrusion Detection Systems) გასასინჯათ, ღია პორტების სანახავად, ქსელირი traffic simulation და როგორც გენერალური TCP/IP ხელსაწყო. TCP/IP-ს სწავლებაშიც გამოგადგებათ.
ამასთანავე შეიძლება რომ გამოდგეს Man In The Middle (MTM) თავდასხმებისთვის, Denial of Service (DoS) თავდასხმებისთვის, verifying if egress/ingress filtering is "on" on the routers.

**მაგალითი 1: ყოველივე პაკეტის ხელში ჩაგდება**

```
packit -m cap
```

**მაგალითი 2: მარტო TCP პაკეტების ხელში ჩაგდება**

```
packit -m cap 'tcp'
```

**მაგალითი 3: მარტო პირველი 100 TCP პაკეტის ხელში ჩაგდება და რეზულტატების ფაილში შენახვა**

```
packit -m cap -c 100 'tcp' -w paketebi.txt
```

-c ანუ count უბრძანებს რომ მარტო 100 პაკეტი დაიჭიროს.
-w ანუ write უბრძანებს რომ მოცემულ ფაილში შეინახოს

**მაგალითი 4: შენახული ფაილის გახსნა**

```
packit -m cap -r paketebi.txt
```

-r ანუ read

### ინექცია

**მაგალითი 5: 100 SYN პაკეტის ინექცია**

```
packit -s 64.73.72.46 -d 134.65.35.65 -S 100 -D 80 -c 100 -F S -e AA:BB:CC:DD:EE:FF
```

-s 64.73.72.46 უბრძანებს რომ source IP იყოს 64.73.72.46
-d 134.65.35.65 უბრძანებს რომ destination IP იყოს 134.65.35.65
-S 100 უბრძანებს რომ source პორტი იყოს 100
-D 80 უბრძანებს რომ destination პორტი იყოს 80
-c 100 უბრძანებს რომ 100 პაკეტი გააგზავნოს
-F S უბრძანებს რომ SYN დროშა აღნიშნოს ყოველ პაკეტში
-e AA:BB:CC:DD:EE:FF უბრძანებს რომ AA:BB:CC:DD:EE:FF გამოიყენოს როგორც source MAC მისამართი

### Trace

**მაგალითი 6: მოქმედებს როგორც traceroute-ი**

უმრავლეს შემთხვევაში მსხვერპლის კომპიუტერის წინა არსებული მოწყობილობა არის როუტერი.

```
packit -m trace -t TCP -d www.site.com -S 80 -F S
```

## სხვა პროგრამები

### ვინდოუსისთვის

#### RafaleX

გრაიფიკული ინტერფეისიანია, გაძლევთ საშუალებას რომ შექმნათ თქვენი პაკეტები
