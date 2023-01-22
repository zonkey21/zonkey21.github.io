# თეორია

# პრაქტიკა

## John the Ripper

John the Ripper-ი პირდაპირ მდ5 ვერ ტეხავს მაგრამ მდ5-ის გატეხვის საშუალების დამატება შესაძლოა ერთერთი პაჩის მიერ.
ეს პაჩი არის john-1.7.2-all-9.diff.gz, და უმატებს სხვა ალგორითმებსაც.

### დაყენება ლინუქსში

```
bash-3.1# mkdir john
bash-3.1# cd john
bash-3.1# wget http://www.openwall.com/john/f/john-1.7.2.tar.bz2
bash-3.1# bunzip2 john-1.7.2.tar.bz2
bash-3.1# tar -xvf john-1.7.2.tar
bash-3.1# cd john-1.7.2
bash-3.1# wget http://distro.ibiblio.org/pub/linux/distributions/openwall/projects/john/contrib/historical/john-1.7.2-all-9.diff.gz
bash-3.1# gzip -d john-1.7.2-all-9.diff.gz
bash-3.1# patch -p1 < john-1.7.2-all-9.diff
bash-3.1# cd src
```

შემდეგი ეტაპია make-ის გაშვება.
როცა make-ს გაუშვებ, არჩევანი გექნება სხვადასხვა ოპერაციული სისტემების და პროცესორების შორის.
უნდა აირჩი შენი კომპიუტერის მიხედვით. ამაზე ნახავ ცოტახანში.

```
bash-3.1# make
To build John the Ripper, type:
        make clean SYSTEM
where SYSTEM can be one of the following:
linux-x86-sse2           Linux, x86 with SSE2 (best)
linux-x86-mmx            Linux, x86 with MMX
linux-x86-any            Linux, x86
linux-x86-64             Linux, AMD x86-64 with SSE2
linux-alpha              Linux, Alpha
linux-sparc              Linux, SPARC 32-bit
linux-ppc32-altivec      Linux, PowerPC w/AltiVec (best)
linux-ppc32              Linux, PowerPC 32-bit
linux-ppc64              Linux, PowerPC 64-bit
freebsd-x86-sse2         FreeBSD, x86 with SSE2 (best)
freebsd-x86-mmx          FreeBSD, x86 with MMX
freebsd-x86-any          FreeBSD, x86
freebsd-x86-64           FreeBSD, AMD x86-64 with SSE2
freebsd-alpha            FreeBSD, Alpha
openbsd-x86-sse2         OpenBSD, x86 with SSE2 (best)
openbsd-x86-mmx          OpenBSD, x86 with MMX
openbsd-x86-any          OpenBSD, x86
openbsd-x86-64           OpenBSD, AMD x86-64 with SSE2
openbsd-alpha            OpenBSD, Alpha
openbsd-sparc64          OpenBSD, SPARC 64-bit (best)
openbsd-sparc            OpenBSD, SPARC 32-bit
openbsd-ppc32            OpenBSD, PowerPC 32-bit
openbsd-ppc64            OpenBSD, PowerPC 64-bit
openbsd-pa-risc          OpenBSD, PA-RISC
openbsd-vax              OpenBSD, VAX
netbsd-sparc64           NetBSD, SPARC 64-bit
netbsd-vax               NetBSD, VAX
solaris-sparc64-cc       Solaris, SPARC V9 64-bit, cc (best)
solaris-sparc64-gcc      Solaris, SPARC V9 64-bit, gcc
solaris-sparcv9-cc       Solaris, SPARC V9 32-bit, cc
solaris-sparcv8-cc       Solaris, SPARC V8 32-bit, cc
solaris-sparc-gcc        Solaris, SPARC 32-bit, gcc
solaris-x86-any          Solaris, x86, gcc
sco-x86-any-gcc          SCO, x86, gcc
sco-x86-any-cc           SCO, x86, cc
tru64-alpha              Tru64 (Digital UNIX, OSF/1), Alpha
aix-ppc32                AIX, PowerPC 32-bit
macosx-ppc32-altivec     Mac OS X, PowerPC w/AltiVec (best)
macosx-ppc32             Mac OS X, PowerPC 32-bit
macosx-ppc64             Mac OS X 10.4+, PowerPC 64-bit
macosx-x86-sse2          Mac OS X, x86 with SSE2
hpux-pa-risc-gcc         HP-UX, PA-RISC, gcc
hpux-pa-risc-cc          HP-UX, PA-RISC, ANSI cc
irix-mips64-r10k         IRIX, MIPS 64-bit (R10K) (best)
irix-mips64              IRIX, MIPS 64-bit
irix-mips32              IRIX, MIPS 32-bit
dos-djgpp-x86-mmx        DOS, DJGPP, x86 with MMX
dos-djgpp-x86-any        DOS, DJGPP, x86
win32-cygwin-x86-sse2    Win32, Cygwin, x86 with SSE2 (best)
win32-cygwin-x86-mmx     Win32, Cygwin, x86 with MMX
win32-cygwin-x86-any     Win32, Cygwin, x86
beos-x86-sse2            BeOS, x86 with SSE2 (best)
beos-x86-mmx             BeOS, x86 with MMX
beos-x86-any             BeOS, x86
generic                  Any other Unix-like system with gcc
```

მე მაქვს ლინუქსი და AMD პროცესორი, რომელიც ექვამდებარება ინტელის x86 პროცესორის არქიტექტურას, ისე რომ თუ ინტელი გაქვს და ლინუქსი გიყენია, ეს წავა.
მაშინ ქენი ასე:

```
bash-3.1# make clean linux-x86-any
```

### მოხმარება

პროგრამა არის დაკომპილირებული კატალოგში როლესაც ქვია run, მაგ კატალოგში რომ შეხვიდე, ქენი ასე:

```
bash-3.1# cd ../run
```

John the Ripper-ი რომ გაუშვა ქენი ასე (თუ run კატალოგში ხარ):

```
bash-3.1# ./john -format=raw-MD5 /home/dzveli_vereli/md5_gasatexi.txt
```

raw-MD5 ანიშნებს რომ შენ ფაილი გაქვს (/home/dzveli_vereli/md5_gasatexi.txt) რომელშიც ინფორმაცია ახწერეილია (მაგალითად) ასე:

Alice:5f4dcc3b5aa765d61d8327deb882cf99
Bob:1c0b76fce779f78f51be339c49445c49

იცოდე რომ გადეხვას დიდი დრო სჩირდება.

მადლობა ინტერნეტს:
http://www.disenchant.ch/blog/teaching-john-the-ripper-how-to-crack-md5-hashes/106/comment-page-1

### საიტები

[[reference:hash_crackers|]]
სხვადასხვა საიტი გთავაზობთ მეტ ნაკლებად სრულ გატეხილი პაროლების სიას.
შეიძლება შენი ნაპოვნი პაროლი ერთერთ სიაში იყოს.

### სხვადასხვა

სკრიპტი პერლში და ფინტონში
[[http://www.googlebig.com/forum/-perl-md5-brute-forcer-t-3782.html#pid4395]]

[[http://b-con.us/pages/md5.php]]

[[http://juggernaut.wikidot.com/jtr | დამატებითი ინფორმაცია John the Ripper-ის გამოყენებისთვის]]



