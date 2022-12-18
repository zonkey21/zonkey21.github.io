# შესავალი

whois (პორტი 43) გაძლევს ინფორმაციას მოცემული დომენის შესახებ. whois პროტოკოლის
RFC არის RFC 3912, [https://www.ietf.org/rfc/rfc3912.txt](https://www.ietf.org/rfc/rfc3912.txt).

ეს ინფორმაციები არის: - ადმინისტრატიული: სახელები, ტელეფონის ნომრები, და
სვადასხვა მისამართები (admin-c, tech-c, zone-c, bill-c…); - ტექნიკური: DNS-ების
სახელები, ელ-ფოსტის მისამართებები (ადმინისტრატორების და სხვების), IP
მისამართებები რომლებიც არიან გამოყოფილნი დამიზნული დომენისთვის.

# მთავარი Whois სერვერების სია

|      Whois     |      ვებ გვერდი  | დომენის გეოგრაფიული ზონა 	|
| 		 | 		    |				|
| APNIC	         | [https://www.apnic.net/about-apnic/whois_search/2/](https://www.apnic.net/about-apnic/whois_search/2/)  | აზია     |
| RIPE           | [https://apps.db.ripe.net/db-web-ui/#/query](https://apps.db.ripe.net/db-web-ui/#/query)     | ევროპა, ახლო აღმოსავლეთის ქვეყნები, ზოგიერთი აზიის და აფრიკის ქვეყნები |
| ARIN           | [http://whois.arin.net/ui](http://whois.arin.net/ui)                           		| ამერიკა |
| InterNIC       | [https://www.internic.net/whois.html](https://www.internic.net/whois.html)                	| .com .net .org .edu |

# მაგალითი

ინფორმაციის მოპოვება დადგენა elitemodel.com-ის შესახებ.

```
sniper@kali ~> whois elitemodel.com
Domain Name: ELITEMODEL.COM
Registry Domain ID: 10224_DOMAIN_COM-VRSN
Registrar WHOIS Server: whois.networksolutions.com
Registrar URL: http://networksolutions.com
Updated Date: 2016-02-17T15:19:33Z
Creation Date: 1996-02-14T05:00:00Z
Registry Expiry Date: 2021-02-15T05:00:00Z
Registrar: Network Solutions, LLC
Registrar IANA ID: 2
Registrar Abuse Contact Email: abuse@web.com
Registrar Abuse Contact Phone: +1.8003337680
Domain Status: clientTransferProhibited
https://icann.org/epp#clientTransferProhibited
Name Server: NS21.WORLDNIC.COM
Name Server: NS22.WORLDNIC.COM
DNSSEC: unsigned
URL of the ICANN Whois
Inaccuracy Complaint Form:
https://www.icann.org/wicf/
>>> Last update of whois
>>> database:
>>> 2019-01-20T13:37:31Z <<<
```

ვხედავთ რომ მოვიპოვეთ ინფორმაცია DNS-ის შესახებ.

აგრეთვე შეგიძლია ინფორმაცია მოიპოვო ვებგვერების მეშვეობით როგორიცაა
[www.whois.net](www.whois.net), [www.internic.net/whois.html](www.internic.net/whois.html) და სხვა.

# საიტები Whois-ის დასადგენად

## VisualRoute

საიტი [http://www.visualroute.com](http://www.visualroute.com) გვაჩვენებს არამარტო გზას ჩვენიდან სხვერპლამე
არამედ რუქაზეც აღნიშნავს მსხვერპლის მდგომარეობას. თან გაჩვენებს ინფორნაციას
საიტის რეგისტრაციის შესახებ. აგრეთვე არსებობს ვინდოუსის პროგრამა VisualRoute
რომელიც იყიდება.

შედი საიტზე, შეიტანე შენი დასამოწმებელი საიტის ადრესი და შედეგიც ეკრანზე
გამოვა. თან გაჩვენებს IP მისამართებს. ამ ინფორმაციის მეშვეობით შეგვიძლია რომ
გავიგოთ თუ რომელი სერვერი ასრულებს როუტერის როლს (უმეტეს შემთხვევაში ეს არის
მსხვერპლის წინა მდგომარე სერვერი).

იცოდე რომ მოცემული რუქის გადიდება შესაძლოა მარცხენა დაწკაპუნებით.

## Sam Spade

საიტი [http://www.samspade.org](http://www.samspade.org) გვაძლევს საშუალებას რომ გავეცნოთ რომელიმე საიტის რეგისტრაციის ინფორმაციებს და რომ დავადგინოთ გზა ჩვენს სასურველ საიტამდე.

# დაცვა
რეგისტრაციის დროს მხოლოდ ზოგადი მონაცემების შეტანა, ინფორმაციის დამალვს თქვენი
registrant სერვისის მიერ.
