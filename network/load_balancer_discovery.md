# lbd (load balancing detector)


lbd არის პროტოტიპის დონის შელ სკრიპტი რომელიც ადგენს დომენი ხმარობს თუ არა load balancing-ს. უკვირდება თუ ამ სისტემით მოქმედებს DNS-ი და HTTP-ე. ამ პროგრამის გამოყენება საინტერესოა დომენის არქიტექტურის დასადგენად, ნუთუ როგორ გაუძლებს უცბათ ტრეფიკის გაზრდას როგორც ეს ხდება DDoS ტიპის შეტევების დროს.

**მაგალითი:**

```
root@bt:/pentest/enumeration/web/lbd# ./lbd.sh reactfeminism.de

lbd - load balancing detector 0.2 - Checks if a given domain uses load-balancing.
                                    Written by Stefan Behte (http://ge.mine.nu)
                                    Proof-of-concept! Might give false positives.

Checking for DNS-Loadbalancing: NOT FOUND
Checking for HTTP-Loadbalancing [Server]: 
 Apache/2.2.9 (Debian) PHP/5.2.6-1+lenny16 with Suhosin-Patch
 NOT FOUND

Checking for HTTP-Loadbalancing [Date]: 19:31:55, 19:31:56, 19:31:56, 19:31:56, 19:31:56, 19:31:56, 19:31:56, 19:31:56, 19:31:56, 19:31:56, 19:31:57, 19:31:57, 19:31:57, 19:31:57, 19:31:57, 19:31:57, 19:31:57, 19:31:57, 19:31:57, 19:31:58, 19:31:58, 19:31:58, 19:31:58, 19:31:58, 19:31:58, 19:31:58, 19:31:58, 19:31:59, 19:31:59, 19:31:59, 19:31:59, 19:31:59, 19:31:59, 19:31:59, 19:31:59, 19:31:59, 19:32:00, 19:32:00, 19:32:00, 19:32:00, 19:32:00, 19:32:00, 19:32:00, 19:32:00, 19:32:00, 19:32:01, 19:32:01, 19:32:01, 19:32:01, 19:32:01, NOT FOUND

Checking for HTTP-Loadbalancing [Diff]: NOT FOUND

reactfeminism.de does NOT use Load-balancing.
```
