# LDAP-ის ინფრომაციის ანონიმურად მოპოვება

```
# ldapsearch -h 192.168.0.65
```

# LDAP-ის უხეში ძალით გატეხვა

თუ ანონიმურად ვერ მოიპოვეთ ინფორმაცია, ალბათ უხეში ძალა გაამართლებს.

```
# bf_ldap

Eliel Sardanons <eliel.sardanons@philips.edu.ar>

Usage:

bf_ldap <parameters> <optional>

parameters:

        -s server

        -d domain name

        -u|-U username | users list file name

        -L|-l passwords list | length of passwords to generate

optional:

        -p port (default 389)

        -v (verbose mode)

        -P Ldap user path (default ,CN=Users,)
```

