# Linux-Server-Configuration
### Introduction
> Setting up a linux server to host our web application,secure it from various attacks complete description of how to setup,configure and deploy our server
### Live version of project
* [Item-catalog](http://ec2-13-126-69-17.ap-south-1.compute.amazonaws.com/)
* public ip address :- 13.126.69.17
## How to configure the linux server
1. ### Setting up development enviroment
* click [here](https://lightsail.aws.amazon.com) to setup a development enviroment.
* create a instance and then you will get a public ip address.
* Download the private key.
2. ### What to do with private key?
* Move the private key in a folder on your machine
* change the access of the private key so only owner can access .
``` 
$ chmod 600 ~/.ssh/key 
```
* ssh into the instance .
 ```
 $ ssh -i ~/.ssh/key.rsa root@public_IP_address
 ```
 3. ### Create a new user
 * command to add user.
 ```
 sudo adduser grader 
 ```
 * try to run .
 ```
 sudo/etc/var
 ```
 * it shows the error because grader has no access to sudo .
 * To give grader access to sudo .
 ```
 $ sudo nano /etc/sudoers.d/grader
 ```
 * add the text to the craeted file
 ```
 grader ALL=(ALL:ALL) ALL
 ```
 * Now again run the command 
 ```
 sudo/etc/var
 ```
 * now the above command will work because now grader have access to sudo
 
