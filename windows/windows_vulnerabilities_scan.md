# WinFingerprint

| ო.ს  	       |  ვინდოუსი  |
| წინა პირობა  | NULL სესია, პორტი 137 UDP, IPX, ან NetBEUI |
| დაცვა        | host based firewall, restrict anonymous |

WinFingerprint არის პროგრამა რომელიც გაჩვენებთ ვინდოუსის რამოდენიბე სისუსტეს.

ეს ინფორმაციებია:
- პორტები
- სერვისები
- დანაყოფები (shares)
- პაროლების განაგების წესები (password policies)
- მყარი დისკის დაყოფები (C:, D:, …)
- MAC მისამართი
- მომხმარებლების ზედმეტი სახელები
- მომხმარებლების SID ID-ები
- მომხმარებლების RID ID-ები
- არსებული ჯგუფების სახელები
- RPC bindings

ბევრ ინფორმაციას ვერ მიიღებთ თუ მსხვერპლმა ანონიმური დაკავშირება (NULL სესია) აკრძალა.

გადმოიწერეთ პროგრამა WinFingerprint.

დასაყენებად დაუწკაპუნე არქივს, შემდეგ მიეცი Next, მერე Yes ყოველ შეკითხვაზე და Finish სულ ბოლოს.

შემდეგ შეასრულეთ სკანირება (პროგრამის გამოყენებას არ გასწავლით ვიმედოვნებ რომ მაგდენად ჩკვიანები ხართ).

თუ ვინდოუსის გაყოფილი რესურსები (Windows Shares) დაფიქსირდა შეიტანეთ ამ რესურსის სრული მისამართი ბრაუზერში და ხელში ჩაიგდებთ მაგ რესურსებს (ფაილი და სხვა).


# LANguard

| ო.ს   | ვინდოუსი  |
| დაცვა | host-based firewalls |

LANguard არა მარტო ღია პორტების ნახვა შეუძლია არამედ სისუსტეების დადგენაც, იგი არის მრავალფეროვანი. მას შეუძლია მოქმედება სკრიპტების მეშვეობით.

ფრთხილად იყავით LANguard მსხვერპლის კომპიუტერს თიშავს თუ მას დაყენებული აქვს ვინდოუსი და Service Pack 2 (Service Pack 1-ზე არ მოქმედებს).

LANguard ბოლო ვერცია ფასიანი გახდა.
