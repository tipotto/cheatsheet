# Hydra
Hydra is a very fast online password cracking tool, which can perform rapid dictionary attacks against more than 50 Protocols, including Telnet, RDP, SSH, FTP, HTTP, HTTPS, SMB, several databases and much more.   

## Normal
```
hydra -t [NUM OF CONNS] -l [USER] -P [WORDLIST] -vV [IP] [PROTOCOL]
```

- -t : Number of parallel connections per target
- -l : Username
- -L : Userlist path
- -p : Password
- -P : Wordlist path
- -s : Port
- -f : Exit when a login/pass pair is found
- -vV : Sets verbose mode to very verbose, shows the login+pass combination for each attempt

## Basic Authentication
```
hydra -l [USER] -P [WORDLIST] [IP] http-get [REQUEST PATH]
```

## Post Form
```
hydra -l [USER] -P [WORDLIST] [IP] http-post-form "[REQUEST PATH]:[REQUEST BODY]:[ERROR MESSAGE]"
```

## Memo
REQUEST PATH : リクエストパスの先頭に/を付けないとエラーになる。  
REQUEST BODY : ユーザー名、パスワードが入る箇所をそれぞれ、^USER^, ^PASS^というマジックパラメータに変更する。