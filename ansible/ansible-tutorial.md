Ansible
=========================


1. get persistant hostname (additionally if you want then map ip to host in OS hosts)
2. Os ping tests (if os ping tests don't work then try with ansible ping)
3. Setup Key based auth for master -> client(password less auth)
4. Ansible Installation (yum -i install ansible )
5. Inventory setup (Ansible hosts entry)
6. Ansible ping test


#Set relevant host name
hostnamectl set-hostname master.example.com 


#for client
hostnamectl set-hostname client.example.com

get ip of machines and add it in hosts file for host to ip mappings
vi /etc/hosts

entry should be in below pattern
<IP> <HLOSTNAME>

Ansible home  : /etc/ansible 

= establish password less authentication from master to client 

ssh-keygen -t rsa (Press enter for default values)

take .pub file content and copy to client (~/.ssh/authorized_keys) ans save the file
now try to access ssh master.exmple.com from master=

/etc/ansible/hosts file has all the IPs of the client
add the client IPs/names here

= ansible ping tests
ansible <hostname> -m ping  //for specific IP

ansible "*" -m ping // for all ips

= you can create group of hosts as well
ansible "grouoName" -m ping 

= docs.ansible.com -->> Module-index for reading module details



==> Ansible ad-hoc commands

by default command module is enabled so no need to pass module name to execute command on clients


[root@master ~]# ansible "*" -a "ls -l /tmp"

[root@master ~]# ansible "*" -a "uname -a" 

[root@master ~]# ansible "*" -a "service ntpd status" // NOt recommended, use service module


[root@master ~]# ansible "*" -m copy -a "src=/root/sachinb.txt dest=/tmp/abc.txt  mode=777 group=sachin "

[root@master ~]# ansible "*" -m user -a "name=sachin uid=1099 state=present"


[root@master ~]# ansible "*" -m user -a "name=sachin uid=1099 state=absent"


[root@master ~]# ansible "*" -m copy -a "src=/root/sachinb.txt dest=/tmp"


[root@master ~]# ansible "*" -m file -a "path=/tmp/abc1.txt  mode=777 group=sachin state=touch"


[root@master ~]# ansible "*" -m service -a "name=nfs state=started"


[root@master ~]# ansible "*" -a "uptime"

[root@master ~]# ansible "*" -m file -a "path=/tmp/abc1/txt  mode=777 group=sachin state=directory"


[root@master ~]# ansible "*" -a "yum install -y telnet" // NOt recommended, use service module



== Understan d YAML

below rules need to be followed 

1. Start of file 
    --- (three hiphen)

2. Indentation (Only spaces not tabs)
3. Key value pair 
4. dashes (Start of new hierarchy)



== creating playbook (Sample)

1. Create playbook (.YAML)

file.yml

- task :
    - name : Installing web server
      yum : name=httpd state=present
    - name : configuring the index page 
      copy : dest=/var/www/html/index.html content="Hello all, I'm from playbook"
    - name : Starting the http service 
      service : name=httpd state=started
...


2. Check the syntax

ansible-playbook file.yml --syntax-check

3. test the playbook
ansible-playbook file.yml --check 

4. run the playbook 
ansible-playbook file.yml
