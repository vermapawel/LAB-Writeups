DC-1 Walkthrough

Lets check whats our IP address is

ifconfig 

![1](images/1.png)

So our IP address is : 192.168.56.101

Lets find the IP address of target machine

nmap -sn 192.168.56.0/24 

-sn → Ping scan (no port scan)

![2](images/2.png)

Target IP address is 192.168.56.102

Now lets perform port scanning using nmap 

nmap -sC -sV -p- 192.168.56.102

-sC will run default NSE scripts

-sV will Detects service versions

-p- will scans all 65535 ports

![3](images/3.png)

On port 80, http is running. NMAP also enumerated that Drupal 7 is running. Drupal is a CMS (Content Management System)

A CMS is software that lets people create, manage, and modify a website without coding.

Lets open port 80

![4](images/4.png)

Lets search if there are any exploits available for Drupal in Metasploit

msfconsole 

![5](images/5.png)

search drupal 7

![6](images/6.png)

Lets use 1st module

![7](images/7.png)




















