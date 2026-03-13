Lab 3 : Intro to EC2

This is elastic compute -> more of computation rather than storage
use Amazon Linux 2023 kernel 6.1 as AMI (Amazon Machine Image)

64 bit (x86) is more intel and slightly outdated -> dominant 5 years back 
64 bit (arm) -> more smart phone based architecture -> consider faster while consuming less energy 

We use 86 for our architecture 

isntance Type : Huge list ! (Something like creating small identifications for one big Data 
First letters stand for family -> stand for different "a" stands for ... "t" family is like basic general smaaller sizes 
number is generation so  t3 is latest t2 is preivous 
SIZE (nano micro ..etc ) every other is double in size as well as cost  


Key Pair :

When you try to connect over internet we ususaly use SSH with passwords and keys like private/ public key you can give private one to server and one to public 

What is security group 
in network -> we want to limit who can connect -> what device do you use to control -> firewall 
security grp is basicaly that -> you can control ports so who can and cannot connect

when you start VM -> you have no idea on which server it runs -> so that server VM has its own local SSD - if you start stop no problem but if you terminate it loses its SSD access 
this is non persistance storage -> basically stop session lose data -> solution EBS - this one have sepecific protoclos (like firewire, Netwwork) to connect storages (Similar to S3 but its a seperate file system) so gives you persistant storage 
Drawback -> 1. slower 
            2. 2 VMs cannot access tha same EBS -> Solution EFS { similar to NFS (Network file system) Shared Network Drive, But EFS is Amazon's}

USER DATA: 
 allows bash script -> runs onces after creating VM 

Line 1 -> installs apache webserver
http-> apache webserver httpd-> http Deamon 

Line2 -> enable deamon Deamons are Background services running (Its not attached to any session so if you close app it wont stop running , there are other managers for deamons)
systemctl is a command line for the httpd

line 4 -> echo '....' {is like printing ....} 
```
#!/bin/bash
dnf install -y httpd
systemctl enable httpd
systemctl start httpd
echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
```

You can edit inbound rules under security group to anywhere IPV4 (allow everyone dont use in real world)


If you want to resize -> first STOP Instance under instance state -> wait to stop-> Actions instance settings -> change instance type -> t2.small
