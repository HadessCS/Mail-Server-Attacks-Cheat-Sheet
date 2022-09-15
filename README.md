# Mail-Server-Attacks-Cheat-Sheet
A cheat sheet that contains common enumeration and attack methods for Mail Server.


## IMAP

```bash
  nmap [-sS] [-sC] -Pn -p 143,993 -sV --script=banner [IP]
```

```bash
  nc -nv <IP> 993 [IP]
```

```bash
  shodan search "port:143"
```
