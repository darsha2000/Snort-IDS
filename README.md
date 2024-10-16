# Snort-IDS(-v 2.x)
Snort is an open-source network intrusion detection and prevention system (IDS/IPS). It analyzes network traffic in real-time, allowing security professionals to detect and block malicious activity. Snort is widely used in cybersecurity to monitor networks for suspicious activities, including attacks, policy violations, and other unauthorized behaviors.

## Diagram

![Screenshot 2024-10-16 200507](https://github.com/user-attachments/assets/dc2738e1-43cc-4cbe-8011-707468888cc4)
### Installation 
- we are using Ubuntu server where we install snort IDS/IPS
- first, we run<br>
```apt-get update && apt-get upgrade```
- then install snort using the command<br>
  ```sudo apt-get install snort -y```
- it will ask for the interface we should define the interface name we can find it using ```ifconfig``` command
- we should set our subnet it may be like 192.168.x.x/24
- the installation will be finished
### step 1
- Turn on the Promiscuous mode so it allows a network interface to receive all traffic on the network
  <br>```sudo ip link set eth0 promisc on```  
- before we run we should configure the snort config file
- command
  <br>```sudo vim /etc/snort/snort.conf```
- to check if snort.conf give any error use the command
  ```sudo snort -c /etc/snort/snort.conf -T```<br>
- in the config file, we should make some changes we should change the HOME_NET to our subnet
- and save
- there will be local.rules file for use to customize the rule or alerts of our own
- to find out our local rules work
- we should comment out all the community rules just add #  at starting of the rules

### step 2
- Use Snorpy to create alerts
- now we open the local.rules file
- path
  <br>```sudo nano /etc/snort/rules/local.rules```
- these are some of the alerts i have used
- for icmp detection<br> ```alert icmp any any  -> $HOME_NET any("ICMp detected "; sid:1000001 ; rev:1;)```
- now run snort using the command
  <br>```sudo snort -q -l /var/log/snort -i ens -A console -c /etc/snort/snort.conf```
  <br>-q is for quick start
  <br>-l is where the log file will be stored
 <br> -i is interface
  <br>-A is alert it have console for real logs fast and quick options so we can analysis the log which are stored
 <br> -c is configuration file is stored
- ping in kali machine using metasploitable 2 ip
- u will now see the alert message in ubuntu server
- we can change $HOME_NET to a certain ip of metasploitable 2 machine
- these are some of alerts i have used and tested alerts
<br>```alert tcp any any -> <dest IP> 22 (msg:"SSH Authentication attempt ";sid:1000002; rev:1;)```<br>
```alert tcp any any -> $HOME_NET 80 (msg:"HTTP GET Request Detected"; "sid:1000003; rev:1;)```
<br>

### step 3
- download  community rules from Snort website
- unzip the community rules and open the rules
- copy any of the community rules and paste to local.rules
- and run
- we can see all the log files in /var/log

