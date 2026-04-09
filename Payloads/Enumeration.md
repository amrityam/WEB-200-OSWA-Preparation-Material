# **Enumeration**

#### **Nmap**
```
sudo nmap -O -Pn 192.168.164.121

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