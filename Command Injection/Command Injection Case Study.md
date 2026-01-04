# **Command Injection Case Study**

## **OpenNetAdmin (ONA)**

### **Exploitation**
#### **Labs**
#### **Lab 1.** Utilize our newly found code execution capability to retrieve a reverse shell, locate and exfiltrate the contents of the flag.txt file located in the web-root of ONA.

Answer - OS{ona_flag1}

- Intercept 'Ping to Verify' request in Burp Suite. 

![case_study_intercept_req](images/case_study_intercept_req.png)

- Check if IP parameter is vulnerabile to command injection.
Payload:
```
xajaxargs[]=ip%3D%3E172.24.0.2;ls
```

![case_study_payload_ls](images/case_study_payload_ls.png)      

- You can see there is a flag.txt file is present. Now read that file.    

![case_study_read_flag](images/case_study_read_flag.png)
