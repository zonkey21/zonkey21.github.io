# პაროლების გამტეხავი

სისტემაში ვეძებთ მომხმარებელს რომლიც სუსტ პაროლს ხმარობს, და მის ანგარიშს ვხმარობს, ამ ხერხით ადმინი ძნელად მიხვდება რომ არალეგიტიმური მომხმარებელის შემოსული სისტემაში.

# ანაბეჭდის (checksum) და თარიღის შემცვლელი

როცა ჰაკერი ცვლის ლეგიტიმურ პროგრამას (ls, ps, du, fsck…) არალეგიტიმურით (ვთქვათ ps, რომ პროცესების სია შეცვალოს) იგი არალეგიტიმური პროგრამის შექმნის/შეცვლის თარირს ამთხვევს ლეგიტიმური პროგრამისას, რომ ცვლილება არ შეიმჩნეს.

აგრეთვე არსებობს ისეთი პროგრამა რომელიც არალეგიტიმური პროგრამის ანაბეჭდს ამთხვევს ლეგიტიმური პროგრამისას.
MD5-ისთვის უფრო რთულია.

# Login ბექდორი

წარმოადგენს ტყოილ პროგრამას რომელიც მომხმარებელს თავაზობს პაროლის დაწერას რომ იგი სისტემაში შევიდეს, ამასობაში კი ამ პაროლს იმახსოვრებს პროგრამა და ხაკერის ხელმისაწვდომი ხდება.

# სერვერის ბექდორი

შეცვლილი სერვერი (FTP…) რომელშიც შეტანილია მექდორის მექანიზმები. ან ისეთი ტყუილი სერვერების გაშვება რიმლებიც არ იბლოკებიან firewall-ის მიერ ხშირ შემთხვევაში (SMTP, DNS, HTTP), აგრეთვე ხშირად დაშვებულია ICMP პროტოკოლი რომლითაც შესაძლოა ინფორმაციების გაგზავნა/გამოგზავნა.

# Cronjob ბექდორი

შესაძლოა crond-ის მეშვეობით ბექდორის გაშვება მხოლოდ გარკვეული დროის განმვალობაში, ასე ადმინი პორტსკანის დროს ვერ წააწყდება ბექდორს თი მაგ დროს არა არის იგი გაშვებული.

# ბიბლიოთეკის ბექდორი

ეხება დიმამიურ ბიბლიოთეკებს. შესაძლოა რომ ჰაკერმა ისე შეცვალოს libc-ს open() ფუნქცია რომ მან ჯერ თავისი ტროჯანი გაუშვას და შემდეგ გახსნას მოთხოვნილი ფაილი, ამ ხერხით პროგრამის MD5 ანაბეჭდი ყოველთვის სწორე იქნება იმიტომ რომ შეცვლილი კოდი არა პროგრამაშია (რომელიც open() ფუნქციას იყენებს) არამედ დინამიურ ბიბლიოთეკაში.

# ბირთვის მიერ ჩასატვირთი მოდული (LKM, Loadable Kernel Module)

მოქბედებს ბირთვის ფენის დონეზე და მაშასადამე აკონტროლებს მთელ სისტემას და მალავს სხვადასხვა ინფორმაციას (პროცესები, ფაილები…).

# პროცესის დამალვა

შესაძლოა argv[0]-ს შეცვლა, ისე რომ ბექდორს არასაეჭვო სახელი ჰქონდეს. ან სახელის შეცვლა ისე რომ ლეგიტიმურად გამოიყურებოდეს (in.syslog).

