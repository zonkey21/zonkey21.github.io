# თეორია

სამიზნე კომპიუტერს ვუგზავნით პაკეტებს გარკვეულ პორტზე, შემდეგ საპასუხოდ მიღებული პაკატების ანალიზს ვახორციელებთ და ვადარებთ მონაცემთა ბაზაში შეტანილ მონაცემებს (ამას ანაბეჭდებას ეძახიან) რომ დავასკვნათ ამ პაკეტების მონაცემები რომელ სერვისს შეეფერება. სხვადასხვა სერვისი პაკეტებში სხვანაირ ინფორმაცია დებს.

# Amap

ფაილი რომელსაც ხმარობს პაკეტების გასაგზავნად არის /etc/apmap/appdefs.trig, შემდეგ რეზულტატებს ადარბს მონაცემებს რომლებიც არის /etc/amap/appdefs.resp ფაილში.

ვიხმაროთ amap-ი ისრ რომ 137-ე პორტზე გაშვებული სერვისის ბანერი (-b) დაბეჭდოს.

```
# amap -b 192.168.1.254 137
```

ამ საქმისთვის აგრეთვე გამოგადგებათ nmap-ი და unicornscan-ი.
