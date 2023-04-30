# Crunch

Crunch ქმნის პაროლების სიებს სხვადასხვა კრიტერიუმების მიხედვით.
შემდეგ ამ სიებს იხმართ პაროლების გასატეხად.

ვებსაიტი: http://sourceforge.net/projects/crunch-wordlist/)

**მაგალითი:** შევქმნათ პაროლების სია 1-დან 5-ასომდე, კომბინაციით

```
hacker@kali ~> crunch 1 5 -o 5aso.txt
Crunch will now generate the following amount of data: 73645520 bytes
70 MB
0 GB
0 TB
0 PB
Crunch will now generate the following number of lines: 12356630 

crunch: 100% completed generating output
```

შევათვალიეროთ რეზულტატი:

```
hacker@kali ~> head 5aso.txt 
a
b
c
d
e
f
g
h
i
j
hacker@kali ~> tail 5aso.txt 
zzzzq
zzzzr
zzzzs
zzzzt
zzzzu
zzzzv
zzzzw
zzzzx
zzzzy
zzzzz
```


**მაგალითი:** შევქმნათ პაროლების სია პატარა ასოებით და რიცხვებით 1-დან 4-სიმბოლომდე

```
hacker@kali ~> crunch 1 4 -f /usr/share/crunch/charset.lst lalpha-numeric -o 1-4_aso_nomrebi.txt
Crunch will now generate the following amount of data: 8588664 bytes
8 MB
0 GB
0 TB
0 PB
Crunch will now generate the following number of lines: 1727604 

crunch: 100% completed generating output
```

შევათვალიეროთ რეზულტატი:

```
hacker@kali ~> head 1-4_aso_nomrebi.txt 
a
b
c
d
e
f
g
h
i
j
hacker@kali ~> tail 1-4_aso_nomrebi.txt 
9990
9991
9992
9993
9994
9995
9996
9997
9998
9999
```

# CeWL

Custom Word List (CeWL) ვებსაიტის URL-ს (Uniform Resource Locator) ეწვის და აგროვებს უნიკალური სიტყვების სიას, ეს სეია შესაძლოა შემდეგ იყოს გამოყენებული როგორც პაროლის სია (wordlist).

ვებსაიტი: http://www.digininja.org/projects/cewl.php

მთავარი პარამეტრებია:
  * depth N ან -d N: ვებგვრდების სიღმე ; დეფოლტია 2
  * min_word_length N ან –m N: სიტყვის მინიმალური სიგრძე; defaultlength არის 3
  * verbose ან –v: ბეჭდავს დამატებ ინფორმაციას
  * write ან –w: რეზულტატს წერს ფაილში

```
hacker@kali ~> cewl -w wordlist.txt http://127.0.0.1/mutillidae
```

შევათვალიეროთ რეზულტატი:

```
hacker@kali ~> tail wordlist.txt 
reported
while
sometimes
throws
errors
databases
Here
case
serious
create
```
