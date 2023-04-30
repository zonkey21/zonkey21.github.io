პაროლის გატეხვა ჩართულ რეჟიმში მოქმედებს მაგალითად ვებსაიტში შესვლის წდით სხვადასხვა მომხმარებლის სახელწოდებით და პაროლით რაც შეიძლება დამცველმა სისტემებმა დაგაფიქსირონ და დაგბლოკონ. ამიტომ პაროლის გატეხვა ჩართულ რეჟიმში ნელად მოქმედებს რომ არ დაფიქსირდეთ.

# Hydra

hydra-ს შეუძლია შემდგომი სერვისების გატეხვა:
adam6500, asterisk, cisco, cisco-enable, cvs, firebird, ftp, ftps, http[s]-{head|get|post}, http[s]-{get|post}-form, http-proxy, http-proxy-urlenum, icq, imap[s], irc, ldap2[s], ldap3[-{cram|digest}md5][s], mssql, mysql, nntp, oracle-listener, oracle-sid, pcanywhere, pcnfs, pop3[s], postgres, radmin2, rdp, redis, rexec, rlogin, rpcap, rsh, rtsp, s7-300, sip, smb, smtp[s], smtp-enum, snmp, socks5, ssh, sshkey, svn, teamspeak, telnet[s], vmauthd, vnc, xmpp.


**მაგალითი:** FTP სერვერის მომხმარებლის admin-ის პაროლის გატეხვა 

```
# hydra -l admin -P passlist.txt ftp://192.168.0.1
```

აგრეთვე არსებობს hydra-ს გრაფიკული ვერცა რომელსაც ქვია xhydra.


# Medusa

Medusa არის ქსელური სერვისების პაროლების გამტეხველი პროგრამა. იგი მოქმედებს პარალელურად, სწრაფად და მოდულარულია. იგი ტეხავს შემდგომ სერვისებს: CVS, FTP, HTTP, IMAP, MS-SQL, MySQL, NCP
(NetWare), PcAnywhere, POP3, PostgreSQL, rexec, Rlogin, rsh, SMB, SMTP (VRFY),
SNMP, SSHv2, SVN, Telnet, VmAuthd, VNC.

გაეცანი განსხვავებებს hydra-ს და medusa-ს შორის: http://foofus.net/goons/jmk/medusa/medusa-compare.html

მთავარი პარამეტრებია:
  * u ან –U [FILE]: მომხმარებლის სახელწოდება ან სახელწოდებების სიის ფაილი.
  * h ან –H [FILE]: hostname ან hostname-მების სიის ფაილი.
  * p ან –P [FILE]: პაროლი ან პაროლობის სიის ფაილი.
  * M: მოდულის სახელწოდება. გამოიყენე -d პარამეტრი მოდულის სახელწოდება.
  * O: რეზულტატის ფაილის სახელწოდება.
  * v: ბეჭდავს დამატებით ინფორმაციას. -v 4 პარამეტრი საინტერესოა.

```
# medusa -u root -P password.lst -h 127.0.0.1 -M vnc -v 4
```
