# **Enumeration**

#### **Nmap**
```
sudo nmap -O -Pn <IP>

sudo nmap -p- -sV -sS -Pn -A <IP>
```
-p- : Scan all 65,535 TCP ports     
-sS : SYN (stealth) scan        
-sV : Service/version detection     
-Pn : Skip host discovery       
-A : Aggressive scan       
-O : Enables operating system detection         


#### **Hakrawler**
```
echo <IP> | hakrawler -u
```
### **Gobuster**
```
gobuster dir -u <IP> -w /usr/share/wordlists/dirb/common.txt
```

```
gobuster dir -u <IP> -w /usr/share/seclists/Discovery/Web-Content/common.txt
```

```
gobuster dir -u <IP> -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files.txt
```
#### Check JS files in a folder
```
gobuster dir -u 192.168.236.125/dev -w /usr/share/seclists/Discovery/Web-Content/common.txt -x js -t 10

gobuster dir -u 192.168.236.125/dev/js -w /usr/share/seclists/Discovery/Web-Content/common.txt -x js -t 10
```

### Use Custom word list generator to bruteforce password
```
cewl -d 2 -m 5 -w custom_passwords_test.txt http://<IP>
```

```
ffuf -w /home/amrityam/kali/offsec/playground/custom_passwords_test.txt -u http://192.168.228.121/dev/index.php -X POST -d 'username=bob&password=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded'
```

### FUZZ GET/POST requests
```
ffuf -c -w /usr/share/seclists/Fuzzing/5-digits-00000-99999.txt -u http://192.168.206.121/scientific/repository/data/FUZZ -e .txt,.pdf,.md -mc 200,301,302 -t 50
```

OR

```
wfuzz -c -z file,/usr/share/seclists/Fuzzing/5-digits-00000-99999.txt -z file,/usr/share/seclists/Fuzzing/extensions-most-common.fuzz.txt --hc 404 http://192.168.189.121/scientific/repository/data/FUZZFUZ2Z
```