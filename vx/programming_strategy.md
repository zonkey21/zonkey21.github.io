ტროას ცხენის ან ჭიის პროგრამისტი დგას სხვადასხვა ტექნიკური არჩევნის წინ, აქ არის არჩევნების სია.

# პროგრამირების ენა

## კომპილირებული

ასემბლერი, C, C++ და სხვა

## ინტერპრეტეტირებული

პითონი, პერლი და სხვა

# პაკეტის "ხელით" შექმნა

  * Raw packet-ებით
  * libpcap/libnet

# ქსელური ტრანსპორტის პროტოკოლი

  * TCP
  * UDP

# ქსელური კავშირი

  * ცენტრალიზებური P2P
  * არა ცენტრალიზებური P2P
  * განსხვავებული პროტოკოლის შექმნა
  * IRC
  * საკავშიროდ დამალული არხის გამოყენება: DNS, HTTP (covert channel)
  * კომპიუტერების განაწილება საქმის პარარელურად გაკეთებისთვის:
    * პორტ სკანირება
    * DDoS
    * სისუსტეების დადგენა, ...

# აიპი მისამართების შექმნა

  * გინდა თუ არა ზოგიერთი აიპი მისამართის მწკრივის აცილება:
    * ამხანაგური საიტები
    * უშიშროება
    * ანტივირუსის კომპანიები
    * ხაფანგი (honeypot)
  * აიპი მისამართების არეულ-დარეულად შექმნა
  * აიპი მისამართების შექმნა გარკვეული მწკრივის ფარგლებში


# სისუსტეების (ბუგების) აღმოჩენა

  * სერვერების სახელობის და ვერსიის დადგენა ექსპლოიტების მოსაძებნად
  * საძიებო სისტემების გამოყენება (გუგლი) ვებ-სისუსტეების მოსაძებნა, ნაპოვნი რესურსების სურათში შენახვა და სტეგანოგრაფია

# ტროას ცხენის/ჭიის payload-ები

## შეკითხვები

  * თავდასხმების განხორციელება დაგეგმილ საათებში თუ არა?
  * შესატევი საიტების მოძებნა საძიებო სისტემით (გუგლი,...) თუ არა?
  * კომპიუტერების დაზომბირება სხვა კომპიუტერების შესატევად?
  * რომელ ოპერაციულ სისტემებზე უნდა იმოქმედოს პროგრამამ?
  * შესატევი საიტების მითითების შესაძლებლობა პროგრამის გავრცელების შემდეგ სასურველია თუ არა?

## ლოკალური ქსელის payload-ები

  * ARP ქეშის მოწამვლა (ARP cache poisoning)
  * Switch-ების ჰაკინგი

### სნიფინგი

  * წინა პირობები: სისტემაში იმდენი უფლება უნდა გქონდეს რომ სნიფინგი შეგეძლოს (root)
  * სნიფერის პროგრამირება შესაძლოა raw socket-ებით ან libpcap/libnet ბიბლიოთეკით

## დისტანციური payload-ები

  * ანგარიშის გატეხვა (brute force)
  * სესიების მოპარვა (session hijacking)
  * სერვერის მხრივ მდებარე სკირპტების სისუსტეების გამოყენება
      * ეს-ქუ-ელ ინექცია
  * .access ფაილის პაროლის გატეხვა
  * DNS-ის ID-ის გაყალბება (DNS ID hijacking)
  * DNS-ის ქეშის მოწამვლა (DNS cache poisoning)
  * War dialing
  * VoIP dialing
  * როუტერების ჰაკინგი
  * DoS/DDoS

## ლოკალური payload-ები

  * Buffer overflow
  * Heap overflow
  * Integer overflow
  * კლიენტის მხრივ მდებარე სკრიპტების სისუსტეების გამოყენება (JavaScript, Java applets, ActiveX, OLE, VBScript...)
  * Diffing

# დაუცველი კომპიუტერების აღმოჩენა

  * პროგრამების შორის აიპი მისამართების განაწილება სკანირებისთვის
  * პორტების სკანირება (connect scan, syn scan, decoy scan...)
  * ოპერაციული სისტემის დადგენა
      * პასიურად
      * აქტიურად
  * ბანერების დადგენა

# შეტევის ფაზა

  * აიპის გაყალბებით (IP spoofing)
  * აიპის გაყალბების გარეშე

# პროგრამის კოპირება

  * კომპილირებული ვერსიის კოპირება (დამოკიდებული იქნებს პროცესორზე და ოპერაციულ სისტემაზე)
  * წყარო კოდით (აგრეთვე შეიძლება კოდის შეცვლა, obsfucation)
  * დაპყრობილი კომპიუტერის ახლიდან დაპრყრობა თუ სისუსტების აღმოფხვრა (patching)
  * USB-ზე კოპირება და Autorun ფაილის კოპირება რომ ვინდოუსში ავტომატურად გაიშვას

# ექსპლოიტები

  * ექსპლოიტების დასაწერად: fuzzing, reverse engineering
  * ექსპლოიტების ოპტიმიზაცია
  * ექსპლოიტის გადაყვანა პოლიმორფიულ ფორმაში (anti-IDS)

# ტროას ცხენის/ჭიის დაცვა (ანტივირუსი, IDS, ფაირვოლი)

  * კოდის წაკითხვის და გაგების გართულება (code obsfucation)
  * შიფრაცია
  * თვით-შიფრაცია
  * პოლიმორფისმი
  * სტეგანოგრაფია
  * თვით-წაშლა
  * კომპილირებული პროგრამის შიფრაცია რომ ანტივირუსმა ვერ აღმოაჩინოს
  * ლოგების წაშლა
    * ლოგ სამართავი პროგრამების გაჩერება
    * ლოგ სამართავი პროგრამების შეცვლა ისე რომ სპეციფიკური მესიჯები კომპიუტერიდან არ გაიგზავნონ
    * ლოგ სერვერზე თავდასზმა
  * ტროას ცხენის/ჭიის ანალიზის გართულება (self-modifying-code)
  * MIME ტიპის შეცვლა (image/xxx)
  * argv[0]-ს შეცვლა რომ ტროას ცხენის/ჭიის ნამდვილი სახელწოდება დამალული იყოს
  * რუტკიტის (rootkit, LKM) პროგრამირება
  * მახეების (honeypots) აღმოჩენა
  * IDS-ის გადალახვა
  * ფაირვოლების გადალახვა (tunneling, covert channel, memory address manipulation)
  * პროქსის გადალახვა
  * გენეტიკური კოდის შექმნა
  * ანტივირუსების პროცესესბის გაჩერება
  * ანტივირუსების საიტების დაბლოკვა იმ მიზნით რომ ახალი ხელმოწერები ვერ მიიღოს
  * Interruptions hijacking
  * ემულატორების არმოჩენა (VirtualBox, VMware, qemu, ...)

# იცოდე სად ხარ

  * მიკროპროცესორის მოდელის დადგენა
  * ქვეყნის დადგენა სადაც ამჟამად იმყოფება ტროას ცხენი/ჭია

# სხვადასხვა

  * გუგლით შეიძლება მოიძებნოს დაუცველი კომპიუტერი და რეზულტატი შევინახოთ სურათის ფაილში რომელიც იქნება დაშიფრული (გასაღებს ვცვლით ყოველივე კავშირის დროს)
  * ალბათობის გამოყენება კომპიუტერებზე თავდასხმისთვის
  * DLL-ის ექსტრაქცია და სხვა პროცესში ინექცია
  * ფუნქციების Hooking-ი
  * ASPack [[www.aspack.com]]
  * egg hunting-ი ანუ მეხსიერების სკანირება მეორე შელკოდის მოსაძებნად, ეს ტექნიკა აღწერილია სტატია [[www.hick.org/code/skape/papers/egghunt-shellcode.pdf|"Safely Searching Process Virtual Address Space"]]
  * თუ პროგრანა ვინდოუსშია გაშვებული შეგვიძლია დავადგინოთ რომელ ქვეყანაში იმყოფება ჭია, GetACP ფუნქციის დაძახებით რომელიც გვაცნობებს კლავიატურის კონფიგურირების კოდს (სხვადასხვა ქვეყანაში სხვადასხვა ტიპის კლავიატურა იხმარება) და რეგისტრში რომ ვნახოთ რა ენაზეა დაყენებული ვინდოუსი SYSTEM\CurrentControlSet\Control\Nls\Language\InstallLanguage
  * ჭიის ახალი ვერსიის დაყენება რომ არ იყოს აკრძალური ანტივირუსისგან შეგვიძლია საიტების თეთრ სიაში შევიტანოთ ჭიის გადმოსაწერი საიტი რეგისტრის მეშვეობით, მაგალითად NOD32-სთვის SOFTWARE\ESET\ESET Security\CurrentVersion\Plugins\01000200\Profiles\@Myprofile\UrlSets\Node_00000000\Masks
  * ვინდოუსში რომ დავმალოთ ჭია შეგვიძლია ავირჩიოთ ფაილის ატრიბუტები როგორიცაა "დამალული ფაილი", "სისტემის ფაილი", "მხოლოდ წაკითხვა" და ექსპლორერში აქტიური გავხადოთ პარამეტრი "არ აჩვენო დამალული ფაილები"
  * ახალი ვერსიის დაყენება შესაძლოა მოხდეს ვინდოუსში ინტერნეტ ექსპლორეში კოდის ინექციით რომ პროცესი ფაირვოლისგან არ დაიბლოკოს.
  * რომ გავარკვიოთ თუ შეგვიძლია ფაილის წაშლა, უნდა გავარკვიოთ რას უდრის UID.
