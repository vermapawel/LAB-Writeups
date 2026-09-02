**Healthcare Walkthrough**

<img width="720" height="277" alt="image" src="https://github.com/user-attachments/assets/f929ae7f-b986-40bb-a3ea-af763045292d" />

Port 21 and port 80 are open. Lets start with FTP first.

<img width="647" height="277" alt="image" src="https://github.com/user-attachments/assets/8e9cd75f-eacd-4072-91f9-0732a50b6e6d" />

Anonymous login is not allowed

Lets bruteforce the directories

***gobuster dir --url http://192.168.1.7 --wordlist /usr/share/dirbuster/wordlists/directory-list-2.3-big.txt***

<img width="720" height="389" alt="image" src="https://github.com/user-attachments/assets/a5a139c9-5ae9-4a64-9697-04c79c3b7ebb" />

We got a login page. Its running OpenEMR v4.1.0

<img width="720" height="403" alt="image" src="https://github.com/user-attachments/assets/fed8ff33-5b83-4983-b7f8-8ef9eb4da3de" />

Lets check if there are any exploit available for OpenEMR

<img width="720" height="175" alt="image" src="https://github.com/user-attachments/assets/ae9fef9b-518f-4e34-87b8-53ae1e0f4243" />

Copy the exploit to local folder and read the exploit

We have to set the correct target

<img width="615" height="247" alt="image" src="https://github.com/user-attachments/assets/e0b72848-0724-4896-ab29-8f142479bff6" />

<img width="720" height="392" alt="image" src="https://github.com/user-attachments/assets/04d9bad0-5a8e-4dfd-b58e-44d49896df2e" />

Lets run the exploit.

<img width="706" height="420" alt="image" src="https://github.com/user-attachments/assets/71584c37-50b2-4c72-93a9-3cba51bbfb8d" />

We got two usernames

admin:3863efef9ee2bfbc51ecdca359c6302bed1389e8

medical:ab24aed5a7c4ad45615cd7e0da816eea39e4895d

Lets try to crack the hash

<img width="720" height="239" alt="image" src="https://github.com/user-attachments/assets/b657388c-7c31-4657-8b08-f5075705d918" />

<img width="720" height="290" alt="image" src="https://github.com/user-attachments/assets/4ba9310d-fa24-4c81-9021-8f86a74084b3" />

admin : ackbar || medical : medical

Lets try to login via admin.

Now we need to find a place where we can upload a reverse shell.

Administration > Files

<img width="720" height="331" alt="image" src="https://github.com/user-attachments/assets/7a556f1b-185c-46a3-9a9f-9502ce81bce6" />

Lets put reverse shell here. Its also showing the path were file will be uploaded.

<img width="720" height="404" alt="image" src="https://github.com/user-attachments/assets/f877ed17-8b4b-4e71-b82c-56fcaef4f243" />

We will start a netcat listener and go to the url where shell is uploaded.

And we got a shell

<img width="720" height="129" alt="image" src="https://github.com/user-attachments/assets/fefd97cf-00be-4912-abfc-cbc575263c4f" />

<img width="720" height="97" alt="image" src="https://github.com/user-attachments/assets/6ea2a6b3-c0f7-4131-97a7-1b7b0552d395" />

Lets check home directory

<img width="702" height="532" alt="image" src="https://github.com/user-attachments/assets/f6f0cb85-fb5e-44f6-a1fa-32df1fee7ea4" />

We got our first flag.

Now we will try to escalate privilege

Lets check SUIDs

***find / -perm -u=s -type f 2>/dev/null***

<img width="622" height="247" alt="image" src="https://github.com/user-attachments/assets/e60b1ec2-4534-4a6b-b0b6-3009aef07279" />

/usr/bin/healthcheck Looks suspicious

<img width="622" height="62" alt="image" src="https://github.com/user-attachments/assets/73795e65-e919-4556-b772-c500df6ddfaa" />

The owner of this file is root. Lets check what this file is doing

strings /usr/bin/healthcheck

<img width="720" height="191" alt="image" src="https://github.com/user-attachments/assets/cb7af38d-142c-4d4f-ae53-0c7abb9d857e" />

There is a script running

In this script, first command that runs is clear (to clear the screen)

Other commands are ifconfig and fdisk -l.

Now, in the script, there is no absolute path for clear command is provided. So it will check all PATH variables where clear command is.

We can create a file with same name, clear, and put /bin/bash in it

cp /bin/bash /tmp/clear

<img width="682" height="97" alt="image" src="https://github.com/user-attachments/assets/c6a4e29c-c2ab-41d3-88fe-314f39337be5" />

We are copying /bin/bash to a file clear inside /tmp folder.

If we set the /tmp before PATH variable, system will start from /tmp folder to find clear command as absolute path for clear command is not provided.

***export PATH=/tmp:$PATH***

<img width="562" height="67" alt="image" src="https://github.com/user-attachments/assets/4c07cb21-22a9-48c8-a0b4-87468c14943b" />

We have already created a fake clear inside /tmp what contains /bin/bash.

System will find fake clear and will execute it.

Lets run the SUID

<img width="720" height="162" alt="image" src="https://github.com/user-attachments/assets/33b5bff7-4b7b-4aab-965d-4c826474b502" />

And we are root

<img width="720" height="193" alt="image" src="https://github.com/user-attachments/assets/c1dbd8b5-506c-4d80-a2f9-712552dd82fb" />

And we got the Root flag !!!!
