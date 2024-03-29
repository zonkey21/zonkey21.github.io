# შესავალი

Fpipe არის ვინდოუსის პროგრამა რომელიც გაძლევთ firewall-ის გადალახვის საშუალებას.

# გამოყენება

ფაირვოლები ფირლტრავენ ქსელის ტრაფიკს. პორტთან დაკავშირების მიმართულების შეცვლა გაძლევთ ამ დაბრკოლების მოგერიების საშუალებას ქსელური კავშირის ფაირვოლის მიერ ნების დართმულ პორტებზე გადაცემით.

## მაგალითი: ვებ-სერვერისთვის განკუთვნილი ტრაფიკის მოძრაობის შეცვლა ლოკალორი კომპიუტერიდან სხვა კომპიუტერამდე

```
fpipe -v -i 192.168.0.3 -l 80 -r 80 92.168.0.20
```

  * -v უბრძანებს რომ პროგრამამ დეტალები გაჩვენოთ 
  * -i უბრძანებს რომ 92.168.0.3 მისამართზე ელოდოს (უსმინოს) დაკავშირებას 
  * -r უბრძანებს რომ remote პორტად გამოიყენოს 80-ე პორტი 80 92.168.0.20 ვებ-სერვერის მისამართია

fpipe ლოკალურ კომპიუტერში 80-ე პორტზე უსმენს და ტრაფიკს გაუგზავნის კომპიუტერს რომელშიც გაშვებულია ვებ-სერვერი.

შეგიძლიათ შეამოწმოთ რომ ლოკალური კომპიუტერი რომელიმე პორტზე ელოდება დაკავშირებას, ასე:

```
netstat -an
```

გაუშვი ბრაუზერი და ჩაწერე ლოკალური კომპიუტერს IP მისამართი (192.168.0.3). რახან ლოკალურ კომპიუტერში 80-ე პორტზე ელოდება კავშირს და fpipe-ი ქსელურ მოძრაობას უცვლის მიმართულებას 80-ე პორტიდან იმ IP მისამართზე სადაც მდგომარეობს ვებ-სერვერი (92.168.0.20), ვებ-გვერდი გამოჩდება.

# დაცვა

ლოგების მონიტორინგი, ძლიერი ACL-ები.
