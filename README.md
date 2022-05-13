# ansible2022

## STEPS TO DO THIS TASK :
- ######   **Ansible Lab setup**
- ######**Configure Docker**
- ######   Start and enable Docker services
- ######   Pull the httpd server image from the Docker Hub
- ######  Launch Container and expose it
- ######   Copy the html code in /var/www/html directory and start the web server.

So, lets start this task .First we have to create Controller Node and Managed Node , As you can see in the below figure i am using My local Redhat VM as Controller Node and Aws Linux instance as Managed Node

![image](https://user-images.githubusercontent.com/56449458/168218971-fac60133-0577-478f-bf45-b71104bcd731.png)

![image](https://user-images.githubusercontent.com/56449458/168219038-f9c7efad-4782-412f-af9e-43e86b52504d.png)


- You can install ansible via pip command as it come from python pip install ansible. after that you can check the version of Ansible.
 

![image](https://user-images.githubusercontent.com/56449458/168219328-a4d7b050-3a85-4b9f-a991-f94cd1b2cd06.png)

- For to configure anything in managed node from the controller node, you must need the IP address, user name, and password of the managed node 
so , lets copy the private key of managed node from base os (windows ) to vm (contraller node )

![image](https://user-images.githubusercontent.com/56449458/168219359-c3a33724-ce2b-4d1e-98c7-f7a695bafbd4.png)

- Then go into the controller node and provide the IP address, user name, and password ( path of where you have the private key of the managed node )
We can create a group of IP in /etc/ansible/hosts, in that you have to write that Managed Node Ip as a host in Controller Node. Give the Username and password (here we using aws instance , so provide path of the private key ) of that Managed Node.

- By creating groups of IP known as Inventory can helps us to do automation that Node which are in group here , As you can see below in the figure, i given awsmn as group name ,If you want to do automation/configuration on multiple Managed Node then you can give their Ip in one of the Group. You can give any name to group
 

![image](https://user-images.githubusercontent.com/56449458/168219551-941c9e59-4216-4980-a3d0-bee49b69cc46.png)


- You can the list of hosts you have. This will give the idea that how many Managed Node we are having. 
            
      ansible all --list-hosts
      
![image](https://user-images.githubusercontent.com/56449458/168219694-b86a3cc0-a343-4205-b5fc-769bf09092b8.png)

- Before running your playbook you need to check that all the Managed Node is connected or pingable with the Controller Node or not. This can be checked by following command

    ansible awsmn -m ping 
(pinging managed nodes which are available in awsmn group with ping module )

![image](https://user-images.githubusercontent.com/56449458/168219785-cb5912db-396d-4b0b-8b04-c582d2bf314e.png)

- Now, we are good to go for creating our own Playbook to run the code. Generally, Ansible Support YAML language in playbook. So, i have created two files one is a playbook dockerce.yml and the other is home.html which we will be copying inside the container

![image](https://user-images.githubusercontent.com/56449458/168219817-d127341e-62a3-4702-ba15-15f04e685f32.png)

- but Now if you run playbook you will get an error telling that the ec2 user not have the power to run some commands

- In Linux Any user that has all the power is also known as privilege user , The command which works in root user or privilege user will not run in the normal user (user you created ) AWS Linux bydefult come with ec2 user (which is known as normal user ) so , ec2 user not have a power to run all the commands


![image](https://user-images.githubusercontent.com/56449458/168219942-83de6020-81db-4c84-9e41-515eca4ed208.png)


- If you want that normal user will have the all the power then it is known as privilege escalation.
- sudo gives you the facility of privilege escalation
- when the Normal user changes his power after running with sudo then is concept is known as becoming
{ become = True } - your user change into root user { become_user = root } - to run your command from the root user you need to become root { become_method = sudo } - Also to becoming root you need to run the command with power of sudo
- so in Ansible configuration file (ansible.cfg) you will have a key word name privilege escalation , just go to there and uncomment the below lines

 ![image](https://user-images.githubusercontent.com/56449458/168220067-26eae72d-85a8-4f2c-aecc-c2b34c236d73.png)
 
- Now ec2-user has root user power , you can check with whoami command

![image](https://user-images.githubusercontent.com/56449458/168220211-f1d4ca93-7fc4-4ebe-87c1-12c6d9e44ccf.png)

- Lets create a plabook for to configure docker-ce and to launch an container with httpd image
code inside dockerce.yml playbook

![image](https://user-images.githubusercontent.com/56449458/168220303-368647c0-d496-4dc1-9356-5b2a70c533fe.png)

  


