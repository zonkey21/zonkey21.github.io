# სერვისების აღმოჩენა portmapper-ით

იუნიქსის ზოგი სერვისები (მაგალითად NFS-ი) გაშვებულია როგორც Remote Procedure Call (RPC) სერვისები, დინამიურად არჩეული პორტებით. რომ კლიენტმა იცოდეს რომელი RPC სერვისია გაშვებული სერვერზე portmapper-ის სერვისი ელოდება კავშირს TCP და UDP პორტ 111-ზე.

RPC portmapper-თან კავშირი შესაძლოა იუნიქსის rpcinfo ბრძანების მეშვეობით.

```
# rpcinfo -p 192.168.0.50
```

# სერვისების აღმოჩენა portmapper-ის გარეშე

ქსელებში რომლებიც ფაირვოლების არიან დაცულნი, RPC portmapper-თან კავშირი ხშირად ფილტრირებულია.

შეგიძლია რომ nmap-ით დაადგინო RPC სერვისები რომლებიც კავშირს ელოდებიან მაღალ პორტებზე თუ portmapper-თან კავშირი არ არის დაშვებული.

```
# nmap -sR 10.0.0.9
```

# rusers

იუნიქსის rusers სერვისი არის ერთ-ერთ Remote Procedure Call (RPC) სერვისი რომელიც კავშირს ელოდება დინამიურად შერჩეულ პორტზე. rusers-კლიენტი ჯერ უკავშირდება RPC portmapper-ს TCP პორტ 111-ზე, რომელიც აგებინებთ rusersd სერვერი გაშვებულია თუ არა.
RPC portmapper ვერ უკავშირდები მაშინ rusers-იც დახურული უნდა იყოს, მაგრამ თუ TCP ან UDP პორტი 111 ღიაა, მაშინ rpcinfo-თი შეგიძლია შეამოწმო rusersd-ის და სხვა RPC სერვისების არსებობა.

## პრაქტიკა

```
# rpcinfo -p 192.168.0.50

program vers proto port  service 

100000   4    tcp  111   rpcbind 
100000   4    udp  111   rpcbind 
100024   1    udp  32772 status 
100024   1    tcp  32771 status 
100021   4    udp  4045  nlockmgr 
100021   2    tcp  4045  nlockmgr 
100005   1    udp  32781 mountd 
100005   1    tcp  32776 mountd 
100003   2    udp  2049  nfs 
100011   1    udp  32822 rquotad 
100002   2    udp  32823 rusersd 
100002   3    tcp  33180 rusersd
```

თუ rusersd აღმოჩენილი იქნა მაშინ ქენით ასე, rusersd-ის მისამართით:

```
# rusers -l 192.168.0.50

Sending broadcast for rusersd protocol version 3...

Sending broadcast for rusersd protocol version 2...

joe      onyx:console            Mar  3 13:03   22:03
george   onyx:ttyp1              Mar  2 07:40
albert   onyx:ttyp5              Mar  2 10:35      14
michael   onyx:ttyp6              Mar  2 10:48
```
