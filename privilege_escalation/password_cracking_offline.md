# hash-identifier

ადგენს პაროლის ჰეშის შიფრაციის სახეობას რომ პაროლი სწორი ალგორითმით გატეხო.

"HASH:"-ის შემდეგ შეგაქვს ჰეში და გაჩვენების ჰეშის სახეობის ალბათობას.

```
hacker@kali ~> hash-identifier 
   #########################################################################
   #	 __  __ 		    __		 ______    _____	   #
   #	/\ \/\ \		   /\ \ 	/\__  _\  /\  _ `\	   #
   #	\ \ \_\ \     __      ____ \ \ \___	\/_/\ \/  \ \ \/\ \	   #
   #	 \ \  _  \  /'__`\   / ,__\ \ \  _ `\	   \ \ \   \ \ \ \ \	   #
   #	  \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \	    \_\ \__ \ \ \_\ \	   #
   #	   \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/	   #
   #	    \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.1 #
   #								 By Zion3R #
   #							www.Blackploit.com #
   #						       Root@Blackploit.com #
   #########################################################################

   -------------------------------------------------------------------------
 HASH: 4dcc4173d80a2817206e196a38f0dbf7850188ff

Possible Hashs:
[+]  SHA-1
[+]  MySQL5 - SHA-1(SHA-1($pass))
```

# Ophcrack

Ophcrack is a rainbow-tables-based password cracker that can be used to crack
the Windows LM and NTLM password hashes. It comes as a command line and
graphical user interface program. Just like the RainbowCrack tool, Ophcrack is based
on the time-memory tradeoff method.

Ophcrack არის წისარტყელის მწკრივზე დაყორნილი (rainbow-tables-based) პაროლების გამტეხველი. მას შეუძლია გატეხოს ვინდოუსის LM ად NTLM პაროლის შეჰები. არსებობს კონსოლის და გრაფიკული ვერცია. მისი ალგორითმი კომპრომისია დროსა და მეხსიერბის რაოდენობის გამოყენებაში.

LM ჰეშები ვინდოუს NT-ამდე არსებობდა, NTLM ჰეშები კი ვინდოუს NT-ს შემდგომ.

კონსოლის ვერცია:

```
ophcrack-cli
```

გრაფიკული ვერცია:

```
ophcrack
```

სანამ ophcrack-ს იხმარ უნდა მოიპოვო წისარტყელური მწკრივევბი (rainbow-tables): http://ophcrack.sourceforge.net/tables.php

მაგალითი: ვინდოუს იქს-პე-ს პაროლის ჰეშის გატეხვა

გადმოვიწერე xp_free_fast წისარტყელური მწკრივი, პაროლის ჰაშია windows_xp_hash ფაილში pwdump ფორმატში.

```
# ophcrack-cli -d fast -t fast -f windows_xp_hash
```
