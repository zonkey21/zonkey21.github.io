VNC იშვება შემგომ TCP პორტებზე:

  * პორტი 5800 VNC-სთან HTTP-თი კავშირისთვის, Java კლიენტით ვებ-გვერდის მეშვეობით
  * პორტი 5900 პირდაპირი კავშირისთვის vncviewer.exe-თი

VNC პაროლი მაქსიმალური სიგრძე არის 8 სიმბოლო.
თუ სერვერი ვინდოუსია, პაროლი შენახულია ვინდოუსის რეგისტრში.

  * \HKEY_CURRENT_USER\Software\ORL\WinVNC3
  * \HKEY_USERS\.DEFAULT\Software\ORL\WinVNC3

VNC-ს პაროლი არის შიფრურებული მყარი გასაღებით, გამოყენებულია DES შიფრაცია.

# იუნიქსი

```
./vncrack -h 192.168.189.120 -w common.txt
```

http://examples.oreilly.com/networksa/tools/vncrack_src.tar.gz

# ვინდოუსი

x4: http://examples.oreilly.com/networksa/tools/x4.exe
