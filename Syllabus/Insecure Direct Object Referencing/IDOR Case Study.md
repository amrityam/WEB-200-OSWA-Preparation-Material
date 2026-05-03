# **Insecure Direct Object Referencing Case Study**

## **Case Study: OpenEMR**

### **Exploiting the IDOR Vulnerability**
#### **Lab 1.** Use the knowledge gained from this Learning Unit and exfiltrate a flag from the vulnerable Case Study endpoint discussed above as our low privileged user.

Answer - OS{ToSeeAWorldInAGrainOfSand}

- Login to http://192.168.199.105 using credentials as username: lowpriv and password: Password1!.

- Go to Message Center tab and click on the patient name, intercept the request in Burp Suite.

![case_study_message_center_patient_page](images/case_study_message_center_patient_page.png)

- The request looks like below, where we can IDOR the noteid parameter.     
GET /interface/patient_file/summary/pnotes_print.php?noteid=11 

- Send the request to intruder, use a number list from 1 to 20 to apply intruder attack for the noteid parameter.

![case_study_intruder_payload](images/case_study_intruder_payload.png)

- Start attack, now for payload 12, you can see the flag.

![case_study_flag_after_intruder_attack](images/case_study_flag_after_intruder_attack.png)


### **Extra Mile**
#### **Lab 2.** One of the patients in OpenEMR is a Systems Administrator. Exfiltrate information from the vulnerable endpoint, retrieve ssh credentials, connect to ssh on port 2222, and read the flag in /var/tmp/flag.txt

Answer - 