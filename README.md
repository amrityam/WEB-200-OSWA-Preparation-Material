
# **WEB-200: OSWA Preparation Material**

A collection of notes to help prepare for the **Offensive Security Web Assessor (OSWA)** certification.

### **Connect VPN**
```
cd /home/amrityam/kali/offsec

sudo openvpn universal.ovpn
```
---

## 📂 **Topics Covered**

### 🛡️ **Introduction to Burp Suite**
- [Burp Suite](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Introduction%20to%20Burp%20Suite/Burp%20Suite.md)
---


### 🛡️ **Web Application Enumeration Methodology**
- [Web Application Enumeration Methodology](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Web%20Application%20Enumeration%20Methodology/Web%20Application%20Enumeration%20Methodology.md)

---
### 🛡️ **Directory Traversal**
- [Directory Traversal - Exploitation](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Directory%20Traversal%20Attacks/Directory%20Traversal%20Attacks.md)

- [Case Study: Home Assistant](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Directory%20Traversal%20Attacks/Directory%20Traversal%20Attacks.md#96-case-study-home-assistant)
---

### 🛡️ **Cross-Site Scripting (XSS)**
- [Cross-Site Scripting - Discovery](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/XSS/Cross-Site%20Scripting%20Introduction%20and%20Discovery/XSS%20Discovery.md)

- [Cross-Site Scripting - Exploitation](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/XSS/Cross-Site%20Scripting%20Exploitation/XSS%20-%20Exploitation.md)

- [Cross-Site Scripting - Case Study](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/XSS/Cross-Site%20Scripting%20Exploitation/XSS%20Case%20Study.md)
---

### 🛡️ **XML External Entities**
- [XML External Entities](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/XML%20External%20Entities/XXE.md)
---


### 🛡️ **Server-side Template Injection (SSTI)**
- [SSTI](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Server-side%20Template%20Injection/SSTI.md)

- [SSTI Case Study](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Server-side%20Template%20Injection/SSTI%20Case%20Study.md)
---


### 🛡️ **Server-side Request Forgery (SSRF)**
- [SSRF](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Server-side%20Request%20Forgery/SSRF.md)

- [SSRF Case Study](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Server-side%20Request%20Forgery/SSRF%20Case%20Study.md)
---


### 🛡️ **Command Injection**
- [Command Injection](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Command%20Injection/Command%20Injection.md)

- [Command Injection Case Study](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Command%20Injection/Command%20Injection%20Case%20Study.md)
---


### 🛡️ **Insecure Direct Object Referencing Case Study**
- [IDOR](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Insecure%20Direct%20Object%20Referencing/IDOR.md)

- [IDOR Case Study](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Insecure%20Direct%20Object%20Referencing/IDOR%20Case%20Study.md)
---

### 🛡️ **SQL Injection**
- [Introduction to SQL](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/SQL%20Injection/Introduction%20to%20SQL.md)

- [SQL Injection](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/SQL%20Injection/SQL%20Injection.md)

- [SQL Injection Case Study](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/SQL%20Injection/SQL%20Injection%20Case%20Study.md)
---

### 🛡️ **Cross-Origin Attacks**
- [Cross-Origin Attacks](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Cross-Origin%20Attacks/Cross-Origin%20Attacks.md)
---

### 🛡️ **Tools**
- [Tools](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Tools/Tools.md)
---


## 🔬 **Challenge Labs**

- [Asio](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Challenge%20Labs/asio/asio.md) - Directory Traversal (Read API token from Spring Boot properties file) + SQL Injection (Use sqlmap to get reverse shell from delete message request or from delete Newsletter subscriptions)

- [Bambi](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Challenge%20Labs/Bambi/bambi.md) - Bruteforce Password using custom wordlist + RCE through Command Injection

- [Piano Protocol](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Challenge%20Labs/Piano%20Protocol/piano%20protocol.md) - SQL Injection on login page to retrieve admin credentials through sqlmap +  Directory traversal to access sensitive files like id_rsa + Gain SSH access using the retrieved private key (port-2222)

- [Bubo](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Challenge%20Labs/Bubo/bubo.md) - IDOR using Fuzz technique (Find hidden PDF file endpoint) + RCE through Command Injection(zipPass)

- [Glaucidium](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Challenge%20Labs/Glaucidium/glaucidium.md) - Create a new guest account + SSRF in the CV upload feature to retrieve sensitive API + Using SSRF call the API to create an admin account using gopher protocol + Mako SSTI to get reverse shell

- [Jubula](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Challenge%20Labs/Jubula/jubula.md) - XSS (session hijacking) + SSTI (RCE)

- [Mentor](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Challenge%20Labs/Mentor/mentor.md) - Blind stored XSS + due httpOnly flag not able to steal cookie + Further enumerate to discover main.js file from dev/js folder which has addUser functionality + using XSS add an admin user + Identify XML export and import feature + Execute an XXE attack to read proof.txt flag

- [Construction](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Challenge%20Labs/Construction/construction.md) XSS (blind XSS) + Command Injection (Chaining XSS to CommandInjection for RCE)

- [Fullmoon](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Challenge%20Labs/Fullmoon/fullmoon.md) - Using SSRF and IDOR, manipulate the screenshot functionality to access restricted admin resources, revealing login credentials. Then use EJS SSTI to perform remote code execution, need to bypass filtering logic of specific keywords such as process, require, exec.

- [Screamin Firehawk](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Challenge%20Labs/Screamin%20Firehawk/screaminfirehawk.md)

- [Solarmedical](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Challenge%20Labs/Solarmedical/solarmedical.md)

---

## Search Flag

#### Linux:
```
find / -type f -name "proof.txt" 2>/dev/null
```

#### Windows CMD:
```
dir C:\proof.txt /s /b 2>nul
or
where /r C:\ proof.txt

# To read the content of proof.txt file
type proof.txt
```

#### Windows PowerShell:
```
Get-ChildItem C:\ -Recurse -File -Filter "proof.txt" -ErrorAction SilentlyContinue
```

#### Command to kill running process if port is already in use:
```
sudo kill -9 $(sudo lsof -t -i :<port>)
```
---

## Useful Payloads
- [Enumeration](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Payloads/Enumeration.md)

- [XSS](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Payloads/XSS.md)

- [SQL Injection](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Payloads/SQL%20Injection.md)

- [Directory Traversal](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Payloads/Directory%20Traversal.md)

- [XML external entity (XXE) injection](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Payloads/XXE.md)

- [Server Side Template Injection (SSTI)](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Payloads/SSTI.md)

- [Server-side Request Forgery (SSRF)](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Payloads/SSRF.md)

- [Command Injection](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Payloads/Command%20Injection.md)

- [CSRF](https://github.com/amrityam/WEB-200-OSWA-Preparation-Material/blob/main/Payloads/CSRF.md)

---
## Practice Portswigger Labs
- [All Labs](https://portswigger.net/web-security/all-labs)

- [Learning Paths](https://portswigger.net/web-security/learning-paths)

---
## Write ups:
**Proving Grounds Boxes**      
- [FunBoxEasyEnum](https://infosecwriteups.com/funbox-7-easyenum-walkthrough-vulnhub-3c1ef0f1c2ef)

- [Inclusiveness](https://cyberarri.com/2024/06/16/inclusiveness-pg-play-writeup/)


- [Potato](https://medium.com/@iaw0ken/offsec-potato-writeup-6fa017f8e45c)

- [Shakabra](https://sanaullahamankorai.medium.com/proving-grounds-play-shakabrah-walkthrou-92fae18c30ac)

- [Sumo](https://www.hackingarticles.in/sumo-1-vulnhub-walkthrough/)

- [Hawat](https://medium.com/@Dpsypher/proving-grounds-practice-hawat-3c4d2c388d7f)

- [Interface](https://medium.com/@pk2212/htb-interface-writeup-walkthrough-edf6f0bfba64)


**Hack The Box**        
- [Pilgrimage](https://ctf.got-hacked.wtf/posts/H4-L0-HTB-Pilgrimage)

- [PC](https://ctf.got-hacked.wtf/posts/H4-L0-HTB-PC/)

- [Soccer](https://ctf.got-hacked.wtf/posts/h4-H0-HTB-Soccer/)

- [Precious](https://ctf.got-hacked.wtf/posts/H4-L0-HTB-Precious/)
---