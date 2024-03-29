# თეორია

ინგლისურად Null session, ეს ნიშნავს ვინდოუსთან ანონიმურად დაკავშირებათ.

## პრაქტიკა

### ანონიმური სესიით დაკავშირება

| წინა პირობა | TCP 139, IPX ან NetBEUI |
| დაცვა       | ACL-ები, restrict anonymous, host-based firewalls |

NULL სესია არის გამოყენებული ვინდოუსში Inter-Process Communication-სგან (IPC) გაყოფილი (shared) რესურსების სანახავად.
დაკავშირების დროს მომხმარებელის სახელი და პაროლი არ არის საჭირო.
NULL სესიით ხაკერს შეუძლია ნახოს ინფორმაცია მომხმარეების შესახებ.

**სინტაქსი:**

```
net use \\Target IP Address\IPC$ ""/u:""
```

**მაგალითი:**

```
C:\Documents and Settings\Administrator>net use \\192.168.142.143\ipc$ ""/u:""
The command completed successfully.
```

წარმარტებულ შემთხვევაში, შეგატყობინებთ: The command completed successfully. თან ეს ყველაფერი არ იწერება ლოგ ფაილში (System Event Log).

### Angry IP

| პლატფორმა   | ვინდოუსი |
| წინა პირობა | NULL სესია |
| დაცვა       | restrict anonymous, host-based firewalls, 139 პორტთან დაკავშირების აკრძალვა, 445 outbound |

Angry IP არის პროგრამა რომელიც გვაძლებს საშუალებას რომ ღია პორტები დავადგინოთ (ამ შემთხვევაში NULL სესია არ არის აუცილებელი) ან კიდე რომ ვინდოუსის shared ressources-ბი ვნახოთ.

პროგრამა როგორ მუშაობს თქვენ თითონ ნახეთ.

თუ სხვა კომპიუტერთან დაკავშირების დროს ზედმეტ სახელს და მაროლს გთხოვთ პროგრამა მაშინ NULL სესიით ცადეთ. თუ კიდე გთხოვთ მაშინ ზედმეტი სახელი და პაროლი უნდა გამოიცნოთ.

### xhydra და hydra

| პლატფორმა   |	იუნიქსი |
| წინა პირობა |	shared ressource უნდა ქონდეს მსხვერპლის კომპიუტერს |
| დაცვა       | Bastion/ servers/workstations, host based firewalls |

hydra-თი დაასკანირე მრავაკი IP მისამართები რომლებსც პორტი 139 აქვთ ღია.
ღია share-ს არ სჭირდება პაროლები რომ დაუკავშირდე, ისე რომ შეგეძლება კოპირება,
გადანაცვლა, წაშლა და შენი მიმატებაც.
რომლებსაც პაროლი სჭირდებათ hydra შეუძლია რომ იგი გატეხოს bruteforce-ით.

ჯერ მაგალითი იქნება xhydra-სთვის.

1. Target tab-ში შეცვალე პორტი 139-ად, ასევე გააქტიურეთ Show Attempts და Be Verbose.

2. Password tab-ში Username-ში ჩაწერეთ ნაპოვნი ადმინისტრატორის სახელი რაც შეეხება პაროლს, შეიტანე ნამდვილი პაროლი (თუ იგი იცი) ან ფაილი რომელშიც შეტანილია პაროლების სია (ერთ ხასზე ერთი პაროლი უნდა უყოს).

3. Start tab-ში, hydra ყოველივე პაროლს სცდის მოცემული მომხმარის სალისთვის. თუ პაროლს გატეხავს იგი მიგამნშნებს.

ეხლა პაროლი რომ იცი როგორ დაუკავშირდე იმ კომპიუტერს?

**ა.** შექმენი პაპკა (mkdir, მაგალითად papka) რომელიც შერწყმული იქნება მსხვერპლის share-თან.

**ბ.** დაამონტაჟე შექმნილი პაპკა მსხვერპლის კომპიუტერთან.

**სინტაქსი:**

```
smbmount "\\\\მსხვერპლის IP\პაპკის სახელი\  შენი სახელი -o \
```

პაპკის სახელს ვიცნობთ პროგრამა LANguard-ის მეშვეობით.

**მაგალითი:**

```
smbmount "\\\\192.168.0.3\secret\  superman -o \
```

შემდეგ გამოვა prompt-ი, აქ უნდა შეიტანო ნაპოვნი ადმინისტრატორის სახელი (მაგალითში იქნება admin).

```
> username=admin
```

შემდეგ მოგთხოვს პაროლს, აკრიფე პაროლი

**გ.** რომ დარწრმუნდე რომ პაპკა დამონტაჟებულია, ქენი:

```
ls -l papka
```

წესით უნდა ხედავდეთ იგივეს რაც მსხვერპლის shared პაპკაშია.

თუ ბრძანებებით გირჩევნია მუშაობა შეგიძლია გამოყენება.

**მაგალითი:**

```
hydra 192.168.0.3 smn -s 139 -v -V -l admin -P parolebis_sia.txt -t 36
```
