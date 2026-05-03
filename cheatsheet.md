# Web Application Vulnerabilities – Notes

## 1. local.txt – JavaScript – XSS

- Mainly making requests:
  - Create accounts
  - Change passwords
  - Get HTML page
  - Cookie
  - main.js

- Use HTB to learn
  - `<script>`

- Look out for:
  - Being reviewed by someone (e.g. contact form)
  - Needs a victim to exploit

- Payload of all things

---

## 2. local.txt – SSRF

### Exploitation
- gopher  
  - eMVee-NL/SSRF2gopher  
  - Gopher protocol is used a lot when exploiting SSRF  
  - This script generates a gopher payload that can be used to submit data to a web form

- Looking for files with credentials
- `file:///etc/passwd`
- `http://box/index.php`

### Look for
- `url=http://localhost/contact.html`
- Upload functionality via URLs
- Able to retrieve output

---

## 3. Directory Traversal

- `/etc/passwd`
- `C:\Windows\win.ini`
- `C:\Windows\boot.ini`
- `C:\Windows\System32\drivers\etc\hosts`

- Recommend CBBH module
- Use `jhadidix` wordlist
- Usually found in parameter  
  - `search=contacts.html`

---

## 4. XXE

- Look for upload functionality
- HTB

---

## 5. SQL Injection

```
sqlmap -r post.txt -p username --os-shell
```
Also try to remove value of parameter or set value as *.
---

## 6. OS Command Injection

- Filters
- Different payloads

`w'h'o'ami`

`/n`

## 7. SSTI

- Template Injection Table – Hackmanit
https://cheatsheet.hackmanit.de/template-injection-table/
- Template Injections – Payloads All The Things
- Google
- HTB
- Displayed somewhere
- Template style