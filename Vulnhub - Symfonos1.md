**Symfonos1 Walkthrough**

<img width="720" height="291" alt="image" src="https://github.com/user-attachments/assets/446ecf6b-c544-4432-ad61-6953a046897f" />

<img width="720" height="161" alt="image" src="https://github.com/user-attachments/assets/5d747529-125d-4bd6-8ba7-6edaf5ad919f" />

<img width="720" height="454" alt="image" src="https://github.com/user-attachments/assets/07bf4aef-bed3-45e3-9f1b-3d4cdabb7c59" />

We can see that Samba is running on port 139. Lets start our enumeration with Samba

smbclient -L //<IP Address>

<img width="720" height="347" alt="image" src="https://github.com/user-attachments/assets/6dba4284-3bc8-4ab1-96c8-c7b49dc58063" />

Default user of Samba is Guest. If we dont put any username, it will take default username i.e Guest. Also if we dont put any password and just press enter, it will take default password i.e Samba.

We have helios and anonymous folders available.

Let try to get into each folder.

smbclient //<ipaddress/foldername>

<img width="718" height="151" alt="image" src="https://github.com/user-attachments/assets/6da645f0-42bc-4919-aa15-ee5bdb8d2670" />

ls >> to list all the files

<img width="720" height="224" alt="image" src="https://github.com/user-attachments/assets/ca462af8-3fe3-4eb4-abd6-c1b4d8a392f7" />

There is a file named attention.txt. Lets download it in our machine.

get <filename>

<img width="720" height="213" alt="image" src="https://github.com/user-attachments/assets/be206d06-4428-4ed9-9ca6-f2e7e9b53cb0" />

Lets check what do we have in attention.txt

<img width="720" height="287" alt="image" src="https://github.com/user-attachments/assets/9b03152c-611a-4c98-9142-b276d86c856f" />

Lets enumerate further with enum4linux

enum4linux <IP Address>

<img width="720" height="116" alt="image" src="https://github.com/user-attachments/assets/a66ab626-5f81-42c4-93a2-0880985982eb" />

<img width="720" height="277" alt="image" src="https://github.com/user-attachments/assets/ad62ddf7-6bcf-49c5-92ac-a70591b0788a" />

In SMB we have an another user called helios and his path is //<IP/helios

Lets try to access it.

<img width="675" height="393" alt="image" src="https://github.com/user-attachments/assets/498d2397-72d2-4479-9adc-b76602524fa5" />

‘qwerty’ works for user helios

<img width="720" height="234" alt="image" src="https://github.com/user-attachments/assets/b9079fca-8460-4859-9b3d-50010549a438" />

We have two txt files. Lets download it and check what they have.

<img width="720" height="174" alt="image" src="https://github.com/user-attachments/assets/0fc44727-90f5-45ae-ab3c-8dff2f7fd010" />

/h3l105 looks like a path. Lets test

<img width="720" height="304" alt="image" src="https://github.com/user-attachments/assets/3c32aba4-8961-4bc5-b149-d642071d5ffe" />

<img width="720" height="401" alt="image" src="https://github.com/user-attachments/assets/7af00eb6-23c7-43cc-9930-ff35b036cd7d" />

It seems like a wordpress cms.

/wp-admin give login for users. Lets try that.

<img width="720" height="365" alt="image" src="https://github.com/user-attachments/assets/064183d4-1f1c-49fe-9e5c-0878339f473e" />

Here we are getting some domain name error. Let add this domain in /etc/hosts file.

<img width="655" height="256" alt="image" src="https://github.com/user-attachments/assets/a66fcc96-30a3-4d0d-8ff5-ab6f40ff5501" />

Lets try again

<img width="720" height="382" alt="image" src="https://github.com/user-attachments/assets/8b567c47-4f7d-4e9f-ae84-26d882eeb510" />

Lets scan this CMS by wpscan

***wpscan -- url ‘http://symfonos.local/h3l105' -e u*** >> to enumurate usernames

wpscan --update >> to upate the database of wpscan

<img width="720" height="492" alt="image" src="https://github.com/user-attachments/assets/aade453b-53f3-4452-a9e1-f2bee0419498" />

<img width="720" height="196" alt="image" src="https://github.com/user-attachments/assets/7ac57add-6caa-4ef5-b9f7-df9707887e3a" />

We have only one user i.e admin.

Lets scan for plugins

wpscan --url ‘http://symfonos.local/h3l105' -e ap>> to enumurate all plugins

<img width="720" height="363" alt="image" src="https://github.com/user-attachments/assets/9abc0886-2a10-461d-88a5-3e6c5fa634a5" />

Two plugins were found mail-masta and site-editor

Lets search these plugs exploit.



















