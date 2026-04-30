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
gobuster dir -u <IP> -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files.txt
```

### Use Custom word list generator to bruteforce password
```
cewl -d 2 -m 5 -w custom_passwords_test.txt http://<IP>
```

```
ffuf -w /home/amrityam/kali/offsec/playground/custom_passwords_test.txt -u http://192.168.228.121/dev/index.php -X POST -d 'username=bob&password=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded'
```