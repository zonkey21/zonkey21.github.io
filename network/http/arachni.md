Arachni არის ჩქარი და მოდულარული ხელსაწყო რომელიც ეძებს ვებ პროგრამების მრავალ სისუსტეებს.

Aarachni მოქმედებს "მწვრთნელის" თვალყურით, ეს იმას ნიშნავს რომ იგი თან სწავლობს როგორ შეამოწმოს მისთვის უცნობი რესურსები.
მას მოჰყბება მრავალი მოდული, 40-ზე მეტი, სხვადასხვა ტესტირების ჩასატარებლად. თუ გსურს შეგიძლია შენი plugin-ები დაუმატო რომ Aarachni-ს ფუნქციონირება შენს გემოვნების და საჭიროების მიხედვით შეცვალო.

Aarchni-ს აგრეთვე შეუძლია პარალელული სკანირება განახორციელოს, ეს ხდება კლუსტერების მეშვეობით, XMLRPC პროტოკოლის დამსახურებით.

# ფრთხილად

Arachni ძალიან სრაფია, ეს ნიშნავს რომ ბევრ პაკეტს აგზავნის, და ამით შეიძლება სერვერი ან მონაცემების ბაზა გათიშოს. ეს რომ თავიდან აიცდინო პარალელური სრედების რაოდენობა მიუთითე ‘-–http-req-limit’ პარანეტრით, თუ არაფერი არ მიუთითე თავისთავად 20 სრედს გაუშვებს.

# მაგალითები

**ჩატვირთავს ყველა მოდულს და შეამოწმებს ყოველივე HTML form-ს, ბმულს და ქუქიებს:**

```
$ arachni http://test.com
```

**ჩატვირთავს ყოველივე მოდულს, საიტის ტესტირებას შეუდგება, შეიმოწმება ბმულები/HTML form-ები/ქუქიები და ქვედა დომენები.
აუდიტის რეზულტატი იქნება შენახული ფაილ test.com.afr-ში:**

```
$ arachni -fv http://test.com --report=afr:outfile=test.com.afr
```

**Arachni Framework Report (.afr) ფაილს სხვა ფორმატში გადაყვანისთვის:**

```
$ arachni --repload=test.com.afr --report=html:outfile=my_report.html
```

**სრული შემოწმება, ხანგძლივია:**

```
$ arachni --load-profile=/var/lib/gems/1.9.2/gems/arachni-0.3/profiles/full.afp http://site.com --report=html:outfile=my_report.html
```

**ან სხვანაირ ფაილის ფორმატში თუ გინდა:**

```
$ arachni --lsrep
```

**ყოველივე xss მოდულის ჩატვირთვა:**

```
$ arachni http://example.net –mods=xss_*
```

**ყოველივე მოდულის ჩატვირთვა რომლის სახელი იწყება audit-ით:**

```
$ arachni http://example.net –mods=audit*
```

**მხოლოდ csrf მოდულის გარეშე:**

```
$ arachni http://example.net –mods=*,-csrf
```

**ყოველივე მოდულის ჩატვირთვა xss მოდულების გარდა:**

```
$ arachni http://example.net –mods=,-xss_*
```

ბმულები:
  * http://resources.infosecinstitute.com/web-application-testing-with-arachni/
  * http://arachni-scanner.com/
  * http://groups.google.com/group/arachni
