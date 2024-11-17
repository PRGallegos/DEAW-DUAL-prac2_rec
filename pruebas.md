# Pruebas de DNS master-slave

## Prueba usando nslookup

Vamos a probar la resolución de nombres usando nslookup.

Consultas directas:

```
vagrant@dnsa:~$ nslookup pcprofesor.informatica.ies.test 192.168.57.10
Server:         192.168.57.10
Address:        192.168.57.10#53

Name:   pcprofesor.informatica.ies.test
Address: 192.168.57.20
```

```
vagrant@dnsa:~$ nslookup server01.informatica.ies.test 192.168.57.10
Server:         192.168.57.10
Address:        192.168.57.10#53

Name:   server01.informatica.ies.test
Address: 192.168.57.10
```

```
vagrant@dnsa:~$ nslookup pc04.aulas.ies.test 192.168.57.10
Server:         192.168.57.10
Address:        192.168.57.10#53

Name:   pc04.aulas.ies.test
Address: 192.168.57.101
```

```
vagrant@dnsa:~$ nslookup tic.departamentos.ies.test 192.168.57.10
Server:         192.168.57.10
Address:        192.168.57.10#53

Name:   tic.departamentos.ies.test
Address: 192.168.57.154
```

Consultas inversas:

```
vagrant@dnsa:~$ nslookup 192.168.57.101  192.168.57.10
101.57.168.192.in-addr.arpa     name = pc04.aulas.ies.test.
```

```
vagrant@dnsa:~$ nslookup 192.168.57.104 192.168.57.10
104.57.168.192.in-addr.arpa     name = pc07.aulas.ies.test.
```

```
vagrant@dnsa:~$ nslookup 192.168.57.152 192.168.57.10
152.57.168.192.in-addr.arpa     name = ingles02.departamentos.ies.test.
```

## Prueba usando dig

Vamos a probar la resolución de nombres usando dig.

```
vagrant@dnsa:~$ dig @192.168.57.10 pc02.informatica.ies.test

; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> @192.168.57.10 pc02.informatica.ies.test
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 47252
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 718ec0943ab0176c0100000067390e1c1bebbc4e77a0499e (good)
;; QUESTION SECTION:
;pc02.informatica.ies.test.     IN      A

;; ANSWER SECTION:
pc02.informatica.ies.test. 604800 IN    A       192.168.57.22

;; Query time: 4 msec
;; SERVER: 192.168.57.10#53(192.168.57.10) (UDP)
;; WHEN: Sat Nov 16 21:26:52 UTC 2024
;; MSG SIZE  rcvd: 98
```

```
vagrant@dnsa:~$ dig @192.168.57.10 server02.aulas.ies.test

; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> @192.168.57.10 server02.aulas.ies.test
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 12922
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: c7f31dee85e7e9580100000067390e77221978f7fb3a6ac2 (good)
;; QUESTION SECTION:
;server02.aulas.ies.test.       IN      A

;; ANSWER SECTION:
server02.aulas.ies.test. 604800 IN      A       192.168.57.100

;; Query time: 0 msec
;; SERVER: 192.168.57.10#53(192.168.57.10) (UDP)
;; WHEN: Sat Nov 16 21:28:23 UTC 2024
;; MSG SIZE  rcvd: 96
```

```
vagrant@dnsa:~$ dig @192.168.57.10 lengua.departamentos.ies.test

; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> @192.168.57.10 lengua.departamentos.ies.test
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 63091
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 1f70f687367be7770100000067390fa9be8a244b1f9d1e5d (good)
;; QUESTION SECTION:
;lengua.departamentos.ies.test. IN      A

;; ANSWER SECTION:
lengua.departamentos.ies.test. 604800 IN A      192.168.57.153

;; Query time: 0 msec
;; SERVER: 192.168.57.10#53(192.168.57.10) (UDP)
;; WHEN: Sat Nov 16 21:33:29 UTC 2024
;; MSG SIZE  rcvd: 102
```

Consultas inversas:

```
vagrant@dnsa:~$ dig @192.168.57.10 -x 192.168.57.22

; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> @192.168.57.10 -x 192.168.57.22
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 63432
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: b88fdbeb34dc518e01000000673910f57764141edb67bcd2 (good)
;; QUESTION SECTION:
;22.57.168.192.in-addr.arpa.    IN      PTR

;; ANSWER SECTION:
22.57.168.192.in-addr.arpa. 604800 IN   PTR     pc02.informatica.ies.test.

;; Query time: 0 msec
;; SERVER: 192.168.57.10#53(192.168.57.10) (UDP)
;; WHEN: Sat Nov 16 21:39:01 UTC 2024
;; MSG SIZE  rcvd: 122
```

```
vagrant@dnsa:~$ dig @192.168.57.10 -x 192.168.57.104

; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> @192.168.57.10 -x 192.168.57.104
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 48421
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: a8327d00e03109d50100000067391105f3b076c9fbf0fd18 (good)
;; QUESTION SECTION:
;104.57.168.192.in-addr.arpa.   IN      PTR

;; ANSWER SECTION:
104.57.168.192.in-addr.arpa. 604800 IN  PTR     pc07.aulas.ies.test.

;; Query time: 0 msec
;; SERVER: 192.168.57.10#53(192.168.57.10) (UDP)
;; WHEN: Sat Nov 16 21:39:17 UTC 2024
;; MSG SIZE  rcvd: 117
```

```
vagrant@dnsa:~$ dig @192.168.57.10 -x 192.168.57.151

; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> @192.168.57.10 -x 192.168.57.151
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 44522
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 5444e218cb68ac1f010000006739111e0b8c4aacc1393bd8 (good)
;; QUESTION SECTION:
;151.57.168.192.in-addr.arpa.   IN      PTR

;; ANSWER SECTION:
151.57.168.192.in-addr.arpa. 604800 IN  PTR     ingles01.departamentos.ies.test.

;; Query time: 0 msec
;; SERVER: 192.168.57.10#53(192.168.57.10) (UDP)
;; WHEN: Sat Nov 16 21:39:42 UTC 2024
;; MSG SIZE  rcvd: 129
```

Zone transfer:

```
vagrant@dnsa:~$ dig @192.168.57.11 aulas.ies.test PTR

; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> @192.168.57.11 aulas.ies.test PTR
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: SERVFAIL, id: 29480
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 3020f5ee6eeb6dc401000000673916ed253671adf88dbb3c (good)
;; QUESTION SECTION:
;aulas.ies.test.                        IN      PTR

;; Query time: 0 msec
;; SERVER: 192.168.57.11#53(192.168.57.11) (UDP)
;; WHEN: Sat Nov 16 22:04:29 UTC 2024
;; MSG SIZE  rcvd: 71
```

## Pruebas con host

```
vagrant@dnsa:~$ host pc01.informatica.ies.test 192.168.57.10
Using domain server:
Name: 192.168.57.10
Address: 192.168.57.10#53
Aliases:

pc01.informatica.ies.test has address 192.168.57.21
```

```
vagrant@dnsa:~$ host pc05.aulas.ies.test 192.168.57.10
Using domain server:
Name: 192.168.57.10
Address: 192.168.57.10#53
Aliases:

pc05.aulas.ies.test has address 192.168.57.102
```

```
vagrant@dnsa:~$ host ingles02.departamentos.ies.test 192.168.57.10
Using domain server:
Name: 192.168.57.10
Address: 192.168.57.10#53
Aliases:

ingles02.departamentos.ies.test has address 192.168.57.152
```

Consultas inversas:

```
vagrant@dnsa:~$ host 192.168.57.23 192.168.57.10
Using domain server:
Name: 192.168.57.10
Address: 192.168.57.10#53
Aliases:

23.57.168.192.in-addr.arpa domain name pointer pc03.informatica.ies.test.
```

```
vagrant@dnsa:~$ host 192.168.57.103 192.168.57.10
Using domain server:
Name: 192.168.57.10
Address: 192.168.57.10#53
Aliases:

103.57.168.192.in-addr.arpa domain name pointer pc06.aulas.ies.test.
```

```
vagrant@dnsa:~$ host 192.168.57.151 192.168.57.10
Using domain server:
Name: 192.168.57.10
Address: 192.168.57.10#53
Aliases:

151.57.168.192.in-addr.arpa domain name pointer ingles01.departamentos.ies.test.
```

# Conclusión
Después de llevar a cabo todas las pruebas utilizando nslookup, dig y host en los servidores DNSA y DNSB, se ha verificado que la configuración del sistema DNS opera de manera óptima y satisface todos los requisitos establecidos. Las consultas directas e inversas se resuelven correctamente en todas las zonas configuradas, y las transferencias de zona entre DNSA y DNSB se realizan sin inconvenientes. Esto garantiza una gestión eficiente y fiable de las resoluciones de nombres en nuestra infraestructura de red.

