**HTB -Legacy Walkthrough**

***nmap -sC -sV -p- 10.129.42.8 — min-rate 10000***

<img width="720" height="468" alt="image" src="https://github.com/user-attachments/assets/5dd41a3f-d203-4ecb-a0ab-c60899fa3923" />

We got to know that its a windows box.

microsoft-ds is running on port 445

Lets google if there any exploits for windows xp microsoft-db.

https://www.rapid7.com/db/modules/exploit/windows/smb/ms08_067_netapi/

<img width="720" height="218" alt="image" src="https://github.com/user-attachments/assets/297efed7-d542-475f-85d3-ad384ad5f24e" />

We find one exploit. This could work as we can see SMB is also running.

Its a metasploit exploit. Lets use this one.

<img width="720" height="179" alt="image" src="https://github.com/user-attachments/assets/a261c97f-3f81-4a28-97fe-c42d84567cd4" />

<img width="720" height="232" alt="image" src="https://github.com/user-attachments/assets/8d2f357e-c6ad-4a4c-8475-88f5cbd874cb" />

We need to set RHOSTS. Its target machine IP address.

We will leave RPORT as its already running 445, default port of SMB

<img width="720" height="163" alt="image" src="https://github.com/user-attachments/assets/0680e97c-14ee-4ca8-b8bb-a8bab8a74d62" />

As we have set the LHOST and RHOST. Lets run the exploit

<img width="720" height="201" alt="image" src="https://github.com/user-attachments/assets/0a7907d8-45a4-4c1d-bc15-382a0e2c673a" />

And we have a Meterpreter session.

<img width="720" height="236" alt="image" src="https://github.com/user-attachments/assets/b9dd659c-2e7c-4855-94ad-69ca0407aade" />

Its a 32 bit windows

<img width="604" height="136" alt="image" src="https://github.com/user-attachments/assets/4083d712-4afe-4538-900e-f900d64b3894" />

We are into C drive now.

<img width="720" height="345" alt="image" src="https://github.com/user-attachments/assets/b016859f-d003-423c-8313-4dcf0f3ec7c2" />

There is a user john.

Lets check further

<img width="720" height="447" alt="image" src="https://github.com/user-attachments/assets/9cad0b61-732f-40d8-bffa-efbfce5a9afa" />

<img width="720" height="267" alt="image" src="https://github.com/user-attachments/assets/9c54409c-01f7-4c56-abef-09447b6d8142" />

We got our user flag

Lets check if we can go to Admin

<img width="720" height="597" alt="image" src="https://github.com/user-attachments/assets/74ee8ec6-132c-4d83-a603-535b724921a2" />

We are in the Admin folder now.

<img width="720" height="258" alt="image" src="https://github.com/user-attachments/assets/e1304ddd-cda7-4914-af20-3af344f51713" />

And we got the root flag as well.
