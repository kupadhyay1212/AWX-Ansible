# How To Install Ansible AWX on Ubuntu with docker

# 1. update the ubuntu server

 sudo apt update && sudo apt -y upgrade

 sudo reboot

# 2. Install asnible

sudo apt install ansible

now verify the ansible version by given below cmd
 
sudo ansible --version

![image](/uploads/56037c104e90a7b4e89a0fc8e34c94fe/image.png)

# 3. Install Docker CE on Ubuntu with given cmd
 
sudo apt -y install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - 
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
 
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
 
sudo apt update
 
sudo apt install docker-ce docker-ce-cli containerd.io

sudo systemctl enable docker

sudo systemctl start docker

sudo systemctl status docker

# 4. Install docker-compose in ubuntu

sudo  curl -s https://api.github.com/repos/docker/compose/releases/latest \
  | grep browser_download_url \
  | grep docker-compose-Linux-x86_64 \
  | cut -d '"' -f 4 \
  | wget -qi -

 chmod +x docker-compose-Linux-x86_64
 
sudo mv docker-compose-Linux-x86_64 /usr/local/bin/docker-compose

 Now see verify the docker compose version by given cmd 

 docker-compose version


![image](/uploads/d5b1ee2e0bb1a3db6302998f12469009/image.png)

# 5. Install Node and NPM
 
sudo apt install -y nodejs npm

sudo npm install npm --global

# 6. Install AWX dependencies
 
sudo git clone --depth 50 https://github.com/ansible/awx.git

sudo cd awx/installer/

# Now generate awx cert key file which is further used in inventory file for encryptions 

sudo pwgen -N 1 -s 30

output key

kgQIYOAaGIOHT1jQLHEx6S3Cox2Z8P

# Now edit the inventory file and uncomment and edit these lines in inventory file 

 vi awx/installer/inventory

dockerhub_base=ansible

awx_task_hostname=awx

awx_web_hostname=awxweb

postgres_data_dir="/var/lib/awx/pgdocker" #location of db

host_port=80

host_port_ssl=443

docker_compose_dir="/var/lib/awx/awxcompose"

pg_username=awx

pg_password=awxpass

pg_database=awx

pg_port=5432

rabbitmq_password=awxpass

rabbitmq_erlang_cookie=cookiemonster

admin_user=admin  #this is the user name of AWX GUI

admin_password=Nirvana3 #this is the password of AWX GUI

create_preload_data=True

secret_key=kgQIYOAaGIOHT1jQLHEx6S3Cox2Z8P

awx_alternate_dns_servers="8.8.8.8,8.8.4.4" #public dns

project_data_dir="/var/lib/awx/projects"  # location for playbooks

# 7. Execute playbook to start the awx containers 

Run ansible-playbook command followed by option -i which tells it the inventory file to use. The name of the playbook file is install.yml

sudo ansible-playbook -i inventory install.yml

# 8. Now see the output of the playbook

Installation output, check if you got any error messages if get error then follow the given below instruction
 
# Note if u get error regarding python or docker and ansible_python_interpreter while running playbook then install this

 sudo pip3 install docker
 
sudo pip3 install docker-compose

# 9. Now again run the playbook and verify the output, error will be resolved.

sudo ansible-playbook -i inventory install.yml

![image](/uploads/ab3afe980d6f4c3659d01a9ced291052/image.png)

# If u get server error in broswer then edit this file and add this line at bottom 

 sudo vi /var/lib/awx/pgdocker/10/data/pg_hba.conf

 host    all             all             0.0.0.0/0               md5

and restart the docker

sudo systemctl restart docker

# 10. Now see the docker containers are up or not 

sudo docker ps
![image](/uploads/73225e390e18b8c46217fecb0bcefe5f/image.png)

ensure the 4 containers are up and running 

# 11. Now broswer IP of the server and login with the awx admin credentials which is mentioned above in the inventory file  
![image](/uploads/805d999a032b0c190e462f0de7c729b9/image.png)

# Now login with given credential 

user_name= admin

password= Nirvana3
