**Production Project on AWS**
============================

**Architecture of Project******
![image](https://github.com/pillakarthik4/AWS-Project-Used-In-Production/assets/130967802/602561de-d868-4de8-aaf6-271b2d2fe86d)

how to create a VPC that you can use for servers in a production environment. To improve resiliency, you deploy the servers in two Availability Zones, by using an Auto Scaling group and an Application Load Balancer. For additional security, you deploy the servers in private subnets. The servers receive requests through the load balancer. The servers can connect to the internet by using a NAT gateway. To improve resiliency, you deploy the NAT gateway in both Availability Zones.

OVERVIEW****
=========
The VPC has two private and public subnets with two Availability Zones.
Each Public subnet contains NAT gateway and Load Balancer Nodes.
The servers run in private subnets, are Launched and terminated by using an Auto Scaling group, and receive traffic from Load Balancer.
The server can connect to the internet by using NAT gateway .

![Screenshot (12)](https://github.com/pillakarthik4/AWS-Project-Used-In-Production/assets/130967802/d9383b87-3693-4a71-aef5-e8adfcc74e69)



**Created Autoscaling group with Launch template with 2 private subnets.  2 Instances are created in EC2.
And created Boston-Host to instance to communicate with 2 instances created by Autoscaling group.**



![Screenshot (13)](https://github.com/pillakarthik4/AWS-Project-Used-In-Production/assets/130967802/ebbfc26a-cd8b-4977-b00d-11b7cc38c453)

//**Created Loadbalancer with target group for private subnet instance to communicate with internet.
Load Balancer placed iN public Subnets.**//


In the terminal copied the Pem.file to the local to the remote machine of the Boston host instance using the commands below :

**hp@DESKTOP-LCDUK55 MINGW64 ~/Downloads (main)
$ scp -i C:/Users/hp/Downloads/pilla.pem C:/Users/hp/Downloads/pilla.pem ubuntu@44.213.89.208:/home/ubuntu**

The authenticity of host '44.213.89.208 (44.213.89.208)' can't be established.
ED25519 key fingerprint is SHA256:OnODdNHkW1E714Q/iX1RlTSDFH9fiPNq64fybZ9mGCE.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:1: ec2-44-213-89-208.compute-1.amazonaws.com
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '44.213.89.208' (ED25519) to the list of known hosts.
pilla.pem                                                                                                   100% 1678     6.4KB/s   00:00

**hp@DESKTOP-LCDUK55 MINGW64 ~/Downloads (main)
$ ssh -i "pilla.pem" ubuntu@44.213.89.208**

Welcome to Ubuntu 22.04.2 LTS (GNU/Linux 5.19.0-1025-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu Aug 10 06:59:26 UTC 2023

  System load:  0.0               Processes:             102
  Usage of /:   20.8% of 7.57GB   Users logged in:       1
  Memory usage: 26%               IPv4 address for eth0: 10.0.31.69
  Swap usage:   0%


Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Thu Aug 10 06:10:49 2023 from 117.217.167.243
**ubuntu@ip-10-0-31-69:~$ ls
pilla.pem
ubuntu@ip-10-0-31-69:~$ ssh -i "pilla.pem" ubuntu@10.0.129.14** (connecting to private subnet instance )
The authenticity of host '10.0.129.14 (10.0.129.14)' can't be established.
ED25519 key fingerprint is SHA256:F+jg15kzs1wcC0n9PikDjJGgpmgoQUwL0GhOEhHEPz4.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.0.129.14' (ED25519) to the list of known hosts.
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for 'pilla.pem' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "pilla.pem": bad permissions
ubuntu@10.0.129.14: Permission denied (publickey).
ubuntu@ip-10-0-31-69:~$ ssh -i pilla.pem ubuntu@10.0.129.14
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for 'pilla.pem' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "pilla.pem": bad permissions
ubuntu@10.0.129.14: Permission denied (publickey).
ubuntu@ip-10-0-31-69:~$ chmod 600 pilla.pem
ubuntu@ip-10-0-31-69:~$  ssh -i pilla.pem ubuntu@10.0.129.14
Welcome to Ubuntu 22.04.2 LTS (GNU/Linux 5.19.0-1025-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu Aug 10 07:06:46 UTC 2023

  System load:  0.0               Processes:             95
  Usage of /:   20.8% of 7.57GB   Users logged in:       0
  Memory usage: 24%               IPv4 address for eth0: 10.0.129.14
  Swap usage:   0%

Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


The list of available updates is more than a week old.
To check for new updates run: sudo apt update


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ubuntu@ip-10-0-129-14:~$ sudo  ssh -i pilla.pem ubuntu@10.0.129.14
Warning: Identity file pilla.pem not accessible: No such file or directory.
The authenticity of host '10.0.129.14 (10.0.129.14)' can't be established.
ED25519 key fingerprint is SHA256:F+jg15kzs1wcC0n9PikDjJGgpmgoQUwL0GhOEhHEPz4.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.0.129.14' (ED25519) to the list of known hosts.
ubuntu@10.0.129.14: Permission denied (publickey).
ubuntu@ip-10-0-129-14:~$
ubuntu@ip-10-0-129-14:~$ nano index.html
ubuntu@ip-10-0-129-14:~$ nano index.html
ubuntu@ip-10-0-129-14:~$ python3 -m http.server 8000
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
10.0.2.77 - - [10/Aug/2023 07:45:48] "GET / HTTP/1.1" 200 -
10.0.28.97 - - [10/Aug/2023 07:45:51] "GET / HTTP/1.1" 200 -


//In one private subnet instance created index.html file ://

With Load Blancer internet traffic will communicate with private subnet instance.



![Uploading Screenshot (14).png…]()






 
