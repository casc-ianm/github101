# Github Exercise week 2-2
In this exercise you will learn how to install Apache webhost and how to install/configure Ansible playbook.

NOTE: before you begin on this exercise, you must have a azure virtual machine running in Ubuntu and have access to internet.

## Exercise: Create an Ubuntu VM in Azure
Open ports for SSH and HTTP
SSH into the VM
Inside the VM, create install ansible
Create a directory called apache_basic in your home directory and change directories into it
Create an ansible playbook name it “install_apache.yml” 
Paste the ff to the file:
```
---
- hosts: localhost
  connection: local
  name: Install the apache web service
  become: yes
  gather_facts: false

  tasks:
    - name: Install latest version of Apache
      <var>: <figure out this line to install apache>

```
Run ansible playbook Access website through a browser
Screenshot your work and send to the group. No need to configure inventory etc.

## Solution

### Allowing ports for SSH,http, and https in azure NSG

1. Open your azure portal
2. At the home page, go to search bar and type **vitual machines**
3. Select your virtual machine then go to **networking** under *settings* tab
4. Take note your **NIC Public IP** You will be needing that to connect to your VM via **SSH** and accessing webpage via **HTTP/HTTPS** later on the exercise  
5. Check the **Inbound port rules** if port 22 (SSH), port 80 and 443 (HTTP/HTTPS) were inside of it. If NOT, click **Add inbound port rule** and do the following:
```
Source: any
Source port ranges: *
Destination: Any
service: HTTP
Action: Allow
Priority: leave it by default
Name: leave it by default

...
then click Add. repeat the process for HTTPS and SSH
```

### connecting to azure virtual machine

1. In your Local Machine, access Azure virtual machine via SSH using the **NIC Public IP**.

1. Once inside the Azure virtual Machine, install **ansible**. Type the following command: 
```
sudo apt-get install ansible -y
```
2. Once Ansible has been installed, go to your */home* directory, create a folder named **apache_basic**
``` 
mkdir apache_basic
```
3. Go to that folder by typing this command:
```
cd apache_basic
```
4. Create a yml file named **install_apache.yml**
```
nano install_apache.yml
```
5. Write the following yml commands:
```
---
- hosts: localhost
  connection: local
  name: Install the apache web service
  become: yes
  gather_facts: false

  tasks:
    - name: Install latest version of Apache
      apt: name=apache2 update_cache=yes state=latest
```
6. Save the **install_apache** file. then exit to nano application

7. On the same directory, run the **install_apache.yml** file with the following commands:
```
ansible-playbook install_apache.yml
```
8. The output should be like this:
```
PLAY [Install the apache web service] *******************************************************************

TASK [Install latest version of Apache] *****************************************************************
changed: [localhost]

PLAY RECAP **********************************************************************************************
localhost                  : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
9. To test if the service of apache2 is running, run this command:
```
sudo systemctl status apache2
```
10. To view the sample page of Apache2, Open a web browser on you local machine and type in the **NIC Public IP** of your Azure Virtual Machine
