# თეორია

TODO

# ბაგების სია

  * http://www.securityfocus.com
  * http://packetstormsecurity.org/files/tags/advisory/
  * http://www.kb.cert.org/vuls/
  * http://cve.mitre.org
  * http://xforce.iss.net

# მრავალფეროვანი ხელსაწყოები

## იუნიქსი ან ვინდოუსი

### nikto

nikto ეძებს სხვადასხვა ნაირ სისუსტეებს ვებ სერვერებში, ითვლება როგორც ერთერთი ყველაზე ძლიერი სკანერი ამ დარგში.

**მაგალითი 1:**

```
nikto -h 195.64.54.64
```

**მაგალითი 2:**

```
nikto -h www.site.com
```

**მაგალითი 3:**

```
nikto -evasion 1 2 3 4 5 6 7 8 -h www.site.com
```

გაითვალისწინე რომ nikto ცრუ დადებით რეზულტატებს მოგცემს ხოლმე.

## იუნიქსი

[w3af](https://zonkey21.github.io/network/http/w3af)

## ვინდოუსი

### WHCC Web Hack Control Center

WHCC (გრაფიკული) ეძებს სხვადასხვა ნაირ სისუსტეებს ვებ სერვერებში, თან შეუძლია რომ nikto-ს სისუსტეების მონაცებების ბაზა გამოიყენოს.
თან გთავაზობთ SQL ინექციას და ბრუტფორს.

### N-Stealth

N-Stealth-ს (ფასიანი და გრაფიკული ინტერფეისით) შეიუძლია ვებ სერვერებში სისუსტეების მოძებნა, შეიცავს 30000 ცნობილი თავდასხმის ექსპლოიტებს ვებ სერვერების მიმართ (უფასო ვერციაშ 20000 ექსპლოიტია).

### TCS CGI Scnner

^ პლატფორმა | ვინდოუსი |
^ წინა პირობა | არაფერი |
^ დაცვა       | Bastion servers/workstations, host-based firewalls, OS updates |

ეს პროგრამა ნახულობს თუ მსხვერპლს უყენია ბუგიანი CGI სკრიპტები თუ არა.


### ASP.NET

ASP.NET არის .NET პლატფორმსთვის შექმნილი ენა, რომელიც ხშირად ვინდიუსზა ნახმარი, დინამიურ ვებ-გვერდების გასაკეთებლად, ამ შემთხვევაში ვებ-გვერდის დაბოლოებაა .aspx-ი.
ხელსაწყო nascan.pl (http://www.digitaloffense.net/dnascan.pl.gz) ადგენს ამ ტიპის საიტების კონფიგურაციას და მოდულების სიას.

```
# ./dnascan.pl http://www.example.org

[*] Sending initial probe request...

[*] Sending path discovery request...

[*] Sending application trace request...

[*] Sending null remoter service request...



[ .NET Configuration Analysis ]



       Server   -> Microsoft-IIS/6.0

  Application   -> /home.aspx

     FilePath   -> D:\example-web\asproot\

   ADNVersion   -> 1.0.3705.288
```

===== WebDAV =====

WebDAV-ი არის HTTP პროტოკოლზე დამოკიდებული სერვისი. რომ გავიგოთ თუ WebDAV-ი ხმარობს SEARCH და PROPFIND HTTP მეთოდებს უნდა გამოვიყენოთ კითხვა OPTIONS / HTTP/1.0.

```
MS-Author-Via: DAV

Content-Length: 0

Accept-Ranges: none

DASL: <DAV:sql>

DAV: 1, 2

Public: OPTIONS, TRACE, GET, HEAD, DELETE, PUT, POST, COPY, MOVE, MKCOL, 

PROPFIND, PROPPATCH, LOCK, UNLOCK, SEARCH

Allow: OPTIONS, TRACE, GET, HEAD, COPY, PROPFIND, SEARCH, LOCK, UNLOCK

Cache-Control: private
```

საინტერესო პროგრამაა: http://code.google.com/p/davtest/

### Microsoft Outlook Web Access

Microsoft Exchange სერვერებს შეუძლიათ შემოგთავაზონ წერილების ნახვა HTTP ან HTTPS პროტოკოლებით, ანუ ვებ-გვერდიდან, ამ სერვისს ქვია Outlook Web Access (OWA).

რომ დაადგინო OWA სერვისი მოქმედებს ვებ-სერვერზე თუ არა საიტის შემდეგ უნდა დაუმატო /owa, /exchange, ან /mail. მაგალითად: site.com/owa.
თუ ეს სერვისი არის მაშინ პაროლების გატეხვა შეიძლება, მაგალითად hydra-თი.


# დაცვა

bastion computer, host-based firewalls, ძლიერი ACL-ები
