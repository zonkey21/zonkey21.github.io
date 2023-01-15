ფუზინგი არის ბუგების არმოყენის ტექნიკა. პროგრამა ელოდება მონაცემებს მაგრამ ჩვეულებრი მონაცემების შეტანის მაგივრად შეიტანთ არ მოლოდნილ მონაცემებს, მაგალიტად რომელიცაა ენკოდირებული სხვა ენებზე, ან შეიცავს ვერს სიბოლოს, ან ძალიან დიდი ზომისაა და სხვა. აღმოჩენილი ბუგების სახეობებია buffer overflows,
format strings, code injections, dangling pointers, race conditions, denial of
service conditions, და სხვა.

ფუნზინგისთვის საჭიროა 6 მთავარი ეტაპი:
  - სამსხვერპლო ობიექტის ამორჩევა (პროგრამა, ვებ აპლიკაცია, ქსელური პროტოკოლი, ...)
  - დაადგინე საიდან შედის მონაცემები (ხელით წერა, ფაილი, ...)
  - ბევრი მონაცემის დამზადება
  - დამზადებული მონაცემების ტესტირება სამსხვერპლოზე
  - რეზულტატის ანალიზი
  - დაუცველი პორციის და ექსპლოიტის დადგენა

გაეცანით დოკუმენტს Fuzzing: Brute Force Vulnerability Discovery presentation საიტზე http://recon.cx/en/f/msutton-fuzzing.ppt.


# BED

Bruteforce Exploit Detector (BED) არის ფუზერი რომელიც ახორციელებს ფუზინგს პროტოკოლების წინააღმდეგ რომ აღმოაჩინოს buffer overflows, format string ბუგები, integer overflows,
DoS-ის პირობები, და სხვა. იგი სინჭავს არჩეულ პროტოკოლს, იმ პროტოკოლის ბრძანებების ხმარებით, პრობლემატიკური მონაცემების გაგზავნით რომ მსხვრეპლი მწყობრიდან გამოვიდეს. მხარდაჭერილი პროტოკოლებია: FTP, SMTP, POP, HTTP, IRC, IMAP, PJL, LPD, FINGER, SOCKS4, და SOCKS5.

```
# bed -h
 BED 0.5 by mjm ( www.codito.de ) & eric ( www.snake-basket.de )

Unknown option: h

 Usage:

 ./bed.pl -s <plugin> -t <target> -p <port> -o <timeout> [ depends on the plugin ]

 <plugin>   = FTP/SMTP/POP/HTTP/IRC/IMAP/PJL/LPD/FINGER/SOCKS4/SOCKS5
 <target>   = Host to check (default: localhost)
 <port>     = Port to connect to (default: standard port)
 <timeout>  = seconds to wait after each test (default: 2 seconds)
 use "./bed.pl -s <plugin>" to obtain the parameters you need for the plugin.

 Only -s is a mandatory switch.
```


# JBroFuzz


