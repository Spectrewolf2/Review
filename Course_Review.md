# Course Review
This review will cover the knowledge and skills you have gained thru out this course. The skills that you will be assessed on is Linux, Cloud Computing, Containerization, Ansible, and Terraform. Follow the directions for the sections below.
# Git
Follow the diections
### Step 1 
  Create a Branch and navagte to it
- Navigate to the directory where they have cloned the repository.
- Create a new branch named after their last name followed by _review (e.g., smith_review):
  ```bash
  git checkout -b lastname_review
### Git WRAP UP 
- At the end of this section, students should be familiar with creating and switching branches in Git. This allows them to work on different features or sections of a project independently, without affecting the main codebase.
# Linux
In this Section you will be asked to do a few tasks. Follow the steps in order.
### Step 1 
- Create these directories
  - ansible
  - terraform
  - screenshots
### Step 2 
- Within the ansible directory create the files listed below
  - main.tf
  - host.ini
  - docker-compose-ollama.yml
- Within the terraform directory create the following files listed below
  - ansible-playbook.yml
  - portainer.sh
### Step 3 
- Using the move command, move the files listed below to the correct directories
  - move main.tf to terraform directory
  - move ansible-playbook.yml to ansible directory
### Step 4 
- Take a screenshot from the CLI that shows the location of the main.tf and playbook.yml (use ls)
  - name this image "linux1"
  - move this image to the screenshots directory
### Step 5
- Using the delete command delete the files listed below
  - portainer.sh
  - docker-compose-ollama.yml
### Linux WRAP UP 
- At the end of this section, students should know how to manage files and directories using basic Linux commands. They should be comfortable creating, moving, and deleting files and directories, which are fundamental skills for any system administrator.

# Cloud Computing
In this section you will be required to Navigate AWS platform and complete the tasks given to you.
### Step 1 
- Create a Linux VM
   - Name the VM Pre-Assessment
   - use Unbuntu 22.04
### Step 2 
- Create a Storage Resourse
  - Name it MyStorage
### Step 3 
- Take a screenshot of the VM and Storage running
  - name this image "cloud1" and "cloud2"
  - move this image to the screenshots directory
### Step 4 
- Create a key pair
  - Name it usethis
  - Save the key to your ansible directory
### Cloud WRAP UP 
- At the end of this section, students should understand how to navigate the AWS platform to set up virtual machines, storage solutions, and security keys. They should be able to configure and deploy a basic cloud infrastructure, essential for modern IT environments.

# Containerization
In this section you will create docker compose files and add code to automate the installation of docker.
### Step 1 
- Navigate to the ansible directory
- create file named docker-compose-ollama.yml
- Input the code listed below into the docker-compose-ollama.yml file
  ```yaml
  version: '3'

  services:
    ollama:
      container_name: ollama
      image: ollama/ollama
      ports:
        - "8000:8000"
      restart: always
   ```
### Step 2 
- Navigate to Terraform directory
- Create file named portainer.sh
- Input the code listed below into the portainer.sh file
  ```bash
  !/bin/bash
  # Install Docker
  sudo apt-get update
  sudo apt-get install -y docker.io

  # Install Portainer
  sudo docker volume create portainer_data
  sudo docker run -d -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
### Container WRAP UP 
At the end of this section, students should be proficient in using Docker and docker-compose through configuration files to deploy and manage containerized applications. This includes understanding how to write and apply docker-compose files to control the behavior of Docker containers.

# Ansible
In this section you will add the code to ensure ansible can run properly.
### Step 1 
- Navigate to Ansible directory
- Create the files listed below
  - docker-compose-portainer.yml
  - docker-compose-wordpress.yml
  - host.ini
### Step 2 
- Input the following code into ansible-playbook.yml
  - ansible-playbook.yml
    ```yaml
    - name: Install required packages
      hosts: all
      become: true
      tasks:
          - name: Ensure /home/user directory exists
          ansible.builtin.file:
            path: /home/user
            state: directory
            owner: admin
            group: admin
            mode: "0755"

        - name: Install Docker
          apt:
            name: docker.io
            state: present

        - name: Install Docker Compose
          shell: curl -fsSL https://get.docker.com -o get-docker.sh && sh get-docker.sh
          args:
            executable: /bin/bash

    - name: Deploy Portainer, WordPress, and Ollama
      hosts: all
      become: true
      vars:
        ansible_ssh_private_key_file: /home/student/ansible/usethis.pem 
      tasks:
        - name: Copy Portainer Docker Compose file
          copy:
            src: docker-compose-portainer.yml
            dest: /home/user/docker-compose-portainer.yml

        - name: Copy WordPress Docker Compose file
          copy:
            src: docker-compose-wordpress.yml
            dest: /home/user/docker-compose-wordpress.yml

        - name: Copy Ollama Docker Compose file
          copy:
            src: docker-compose-ollama.yml
            dest: /home/user/docker-compose-ollama.yml

        - name: Start Portainer container
          command: docker-compose -f /home/user/docker-compose-portainer.yml up -d

        - name: Start WordPress container
          command: docker-compose -f /home/user/docker-compose-wordpress.yml up -d

        - name: Start Ollama container
          command: docker-compose -f /home/user/docker-compose-ollama.yml up -d
  - docker-compose-portainer.yml
    ```yaml
    version: '3'

    services:
      portainer:
        container_name: portainer
        image: portainer/portainer
        ports:
          - "9000:9000"
        restart: always
  - docker-compose-wordpress.yml
    ```yaml
    version: '3'

    services:
      wordpress:
        container_name: wordpress
        image: wordpress
        ports:
          - "8080:80"
        restart: always
        environment:
          WORDPRESS_DB_HOST: "wordpress-db:3306"
          WORDPRESS_DB_NAME: "wordpress"
          WORDPRESS_DB_USER: "admin"
          WORDPRESS_DB_PASSWORD: "password"
        depends_on:
          - wordpress-db

     wordpress-db:
        container_name: wordpress-db
        image: mysql:5.7
        environment:
          MYSQL_ROOT_PASSWORD: "password"
          MYSQL_DATABASE: "wordpress"
          MYSQL_USER: "admin"
          MYSQL_PASSWORD: "password"
        volumes:
          - wordpress-db:/var/lib/mysql
        restart: always
 
    volumes:
      wordpress-db:
   - hosts.ini
     ```yaml
     # Define individual hosts
     web1 ansible_host=your ip ansible_user=admin ansible_ssh_private_key_file=/your/path/usethis.pem
  
     web2 ansible_host=your ip ansible_user=admin ansible_ssh_private_key_file=your/path/usethis.pem
  
     [db_servers]
     db1 ansible_host=your ip ansible_user=admin ansible_ssh_private_key_file=/home/student/ansible/usethis.pem
    - you will have to change the IPs and the path to you key that you generated later
### Ansible WRAP UP ###
- At the end of this section, students should have a firm grasp on using Ansible for configuration management and application deployment. They should know how to create Ansible playbooks to automate the deployment of Docker containers and configure environments consistently and reliably.

# Terraform
In this Section you will be asked to show your skills that you have gained
### Step 1
- Navigate to Terraform directory
- Add the Following files
  - wordpress.sh
  - ollama.sh
### Step 2
- Input the following information into the correct files
  - main.tf
    ```hcl
    provider "aws" {
      region  = "us-east-1"  # Specify your desired AWS region here
    }

    resource "aws_instance" "portainer" {
      ami           = "ami-058bd2d568351da34"  # AMI for Debian 10 in us-east-1
      instance_type = "t2.medium"
      key_name      = "usethis"     # Specify your key pair name here

      user_data     = file("portainer.sh")     # User data script for startup
    }

    resource "aws_instance" "wordpress" {
      ami           = "ami-058bd2d568351da34"  # AMI for Debian 10 in us-east-1
      instance_type = "t2.medium"
      key_name      = "usethis"     # Specify your key pair name here

      user_data     = file("wordpress.sh")     # User data script for startup
    }

    resource "aws_instance" "ollama" {
      ami           = "ami-058bd2d568351da34"  # AMI for Debian 10 in us-east-1
      instance_type = "t2.medium"
      key_name      = "usethis"     # Specify your key pair name here

      user_data     = file("ollama.sh")        # User data script for startup
    }
   - ollama.sh
     ```bash
     #!/bin/bash
  
     # Install Docker
     sudo apt-get update
     sudo apt-get install -y docker.io
  
     # Install Ollama
     sudo docker run -d -p 8000:8000 --name=ollama --restart=always ollama/ollama
    - wordpress.sh
      ```bash
      #!/bin/bash
  
      # Install Docker
      sudo apt-get update
      sudo apt-get install -y docker.io

    # Install WordPress
    sudo docker volume create wordpress_data
    sudo docker run -d -p 8080:80 --name=wordpress --restart=always -v wordpress_data:/var/www/html -e WORDPRESS_DB_HOST=wordpress-db:3306 -e WORDPRESS_DB_NAME=wordpress -e WORDPRESS_DB_USER=admin -e WORDPRESS_DB_PASSWORD=password wordpress
### Terraform WRAP UP ###
At the end of this section, students should be able to use Terraform to automate the provisioning of infrastructure on cloud platforms like AWS. They should understand how to write Terraform configuration files and use them to manage infrastructure as code, ensuring that deployments are reproducible and scalable.
# Git
Follow the diections
### Step 1 
  Navigate to Review directory
  Add Changes to the Staging Area:
- Ensure you are still in your custom branch "git branch"
- Add all changes/files/images to the staging area
  ```bash
  git add .
### Step 2
  Commit the changes with an appropriate commit message 
  ```bash
  git commit -m "Complete course review tasks"
```
### Step 3
  Push their branch to the remote repository
  ```bash
  git push origin lastname_review
```
### Git WRAP UP 
- At the end of this section, students should be able to push changes from their local repository to a remote repository. This is crucial for backing up their work and collaborating with others, as it integrates their contributions into the shared project.
    
# Put it all together
- Use terraform to make instances in the cloud using the correct code
  - After The instances were created Navigate to the security groups
    - Ensure Port 22 is open for SSH
- Take the IPs that terraform generated and add them to you Ansible Playbook
  - Ensure You have the correct path for the key you generated from AWS
- Have Ansible create the containers
