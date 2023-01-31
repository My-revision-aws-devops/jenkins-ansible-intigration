#  *JENKINNS - ANSIBLE - INTEGRATION* 
![jenkins-ansible-integration](images/001.PNG)
*** 
##  *steps to follow*
---
1. First thing to do is ansible master-node configurationAnsible
   ---

   1. Take an ec2 instance and make it as ansible master by followiing below steps.
   2. after log into your virtual machine, create a user with password and enable password authentication to the user.
      ```
      $ sudo apt update
      $ sudo adduser <user-name>

      ```
      ![preview](images/010.PNG)
   3. give sudo privileges to the user.
      ```
      $ sudo vi /etc/sudoers

      ```
      ![preview](images/011.png)

   4. enabling password authentication.
      ```
      $ sudo vi /etc/ssh/sshd_config

      ```
      ![preview](images/012.png)
   5. restart sshd service
      ```
      $ sudo systemctl restart sshd

      ```
   6. Repeat all the above steps in ansible node vm .
   7. Now login to the user in ansible master with password.
      ```
      $ ssh <username>@publicIP

      ``` 
      ![preview](images/013.PNG)
      It asks for password.enter  there.
   8. after successful login using password you see this.
      ![preview](images/014.PNG)  
   9. now create rsa key.
      ```
      $ ssh-keygen
      
      ```
      ![preview](images/015.PNG)
   10. now copy this RSA key to ansible node.
       ```
       $ ssh-copy-id <ansible-node-user-name>@<ansible-node-ip>
       
       ```
       it request for password.
       ![preview](images/016.PNG)  
   11. After successful key sharing. we will see this message.
       ![preview](images/017.PNG)
   12. Now try to login to ansible node.
       ```
       $  ssh <ansible-node-user-name>@<ansible-node-ip>

       ```
       it will login with out asking for password.
       ![preview](images/018.PNG)
   13. now install ansible on Ansible master node.
       ```
       $ sudo apt update
       $ sudo apt install software-properties-common
       $ sudo add-apt-repository --yes --update ppa:ansible/ansible
       $ sudo apt install ansible
       $ ansible --version

       ```  
       ![preview](images/020.PNG)
   14. Now create a hosts file with ansible node ip.
       ```
       vi hosts
       ```
       ![preview](images/019.PNG)  
   15. Now ping with ansible node.
       ![preview](images/021.PNG) 
   16. Now install java11 on Ansible master node. 
       ```
        $ sudo apt update
        $ sudo apt install openjdk-11-jdk -y

       ``` 
   17. Now Ansible master node is ready to integrate with Jenkis Master node.  
   
2. Take another EC2 instances
    ---
    1. Do the following configuration.
    2. install java-11
        ```
        $ sudo apt update
        $ sudo apt install openjdk-11-jdk -y

        ```
    3. install jenkins
        ```
        $ curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
          /usr/share/keyrings/jenkins-keyring.asc > /dev/null
        
        $ echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
          https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
          /etc/apt/sources.list.d/jenkins.list > /dev/null
        
        $ sudo apt-get update

        $ sudo apt-get install jenkins -y

        ```
    4. Now give sudo permission to jenkins.
       ```
        $ sudo vi /etc/sudoers

       ```
       ![preview](images/002.png)

    5. Now open jenkins UI in browser.
        ```
        http://<publicIP>:8080
        ```
        ![preview](images/003.PNG)

    6. Now we have to unlock jenkins.
    7. you will find initial password here
        ![preview](images/004.PNG)
       
       ```
       $ sudo vi /var/lib/jenkins/secrets/initialAdminPassword

       ```
       ![preview](/images/005.PNG)
    8. now enter initial password in jenkins ui.
        ![preview](images/006.PNG)
    9. click on install suggested plugins.
        ![preview](images/007.PNG)
    10. create Admin user 
        ![preview](images/008.PNG)
    11. click start using Jeninks
        ![preview](images/009.PNG)
    12. After sucessfull login.  you get this page.
        ![preview](images/022.PNG)
    13. For making Node configuration follow the below steps.
        * click on > manage jenkins
          ![preview](images/023.png)
        * click on > Manage Nodes and Clouds
          ![preview](images/024.png) 
        * click on > New Node
          ![preview](images/025.png)
        * enter node name , select permanent Agent and click on create button.
          ![preview](images/026.png)
        * fill the following configuration details
          ![preview](images/027.png)
          ![preview](images/028.png)
          for adding credentials do follow the steps.
          ![preview](images/029.png)
          ![preview](images/030.png)
        * After successful node configuration you will find node here.
          ![preview](images/031.png)
        * Now creating Declarative pipeline to install apache through Ansible play book.
        * for declarative pipeline, playbook and hosts file refer the git repository.[link](https://github.com/My-revision-aws-devops/jenkins-ansible-intigration.git)
        * Follow the below steps
          ![preview](images/032.png)
          ![preview](images/033.png)
          ![preview](images/034.png)
          ![preview](images/035.png)
          ![preview](images/036.png)
          ![preview](images/037.png)
        * to check status of the build click here
          ![preview](images/038.png)
        * click on console output
          ![preview](images/039.png)
          ```  
            Started by user satya
            Obtained Jenkinsfile from git https://github.com/My-revision-aws-devops/jenkins-ansible-intigration.git
            [Pipeline] Start of Pipeline
            [Pipeline] node
            Running on Ansible_Master in /home/ansible/remote_root/workspace/apache_Installation
            [Pipeline] {
            [Pipeline] stage
            [Pipeline] { (Declarative: Checkout SCM)
            [Pipeline] checkout
            Selected Git installation does not exist. Using Default
            The recommended git tool is: NONE
            No credentials specified
            Cloning the remote Git repository
            Cloning repository https://github.com/My-revision-aws-devops/jenkins-ansible-intigration.git
            > git init /home/ansible/remote_root/workspace/apache_Installation # timeout=10
            Fetching upstream changes from https://github.com/My-revision-aws-devops/jenkins-ansible-intigration.git
            > git --version # timeout=10
            > git --version # 'git version 2.34.1'
            > git fetch --tags --force --progress -- https://github.com/My-revision-aws-devops/jenkins-ansible-intigration.git +refs/heads/*:refs/remotes/origin/* # timeout=10
            > git config remote.origin.url https://github.com/My-revision-aws-devops/jenkins-ansible-intigration.git # timeout=10
            > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
            Avoid second fetch
            Checking out Revision 4bd5e46bfe80fbc3e4cf0dec8df1f1c033b61c29 (refs/remotes/origin/main)
            > git rev-parse refs/remotes/origin/main^{commit} # timeout=10
            > git config core.sparsecheckout # timeout=10
            > git checkout -f 4bd5e46bfe80fbc3e4cf0dec8df1f1c033b61c29 # timeout=10
            Commit message: "3rd commit till build"
            First time build. Skipping changelog.
            [Pipeline] }
            [Pipeline] // stage
            [Pipeline] withEnv
            [Pipeline] {
            [Pipeline] stage
            [Pipeline] { (vcs)
            [Pipeline] git
            Selected Git installation does not exist. Using Default
            The recommended git tool is: NONE
            No credentials specified
            Fetching changes from the remote Git repository
            Checking out Revision 4bd5e46bfe80fbc3e4cf0dec8df1f1c033b61c29 (refs/remotes/origin/main)
            Commit message: "3rd commit till build"
            Post stage
            [Pipeline] echo
            ========A executed successfully========
            [Pipeline] }
            > git rev-parse --resolve-git-dir /home/ansible/remote_root/workspace/apache_Installation/.git # timeout=10
            > git config remote.origin.url https://github.com/My-revision-aws-devops/jenkins-ansible-intigration.git # timeout=10
            Fetching upstream changes from https://github.com/My-revision-aws-devops/jenkins-ansible-intigration.git
            > git --version # timeout=10
            > git --version # 'git version 2.34.1'
            > git fetch --tags --force --progress -- https://github.com/My-revision-aws-devops/jenkins-ansible-intigration.git +refs/heads/*:refs/remotes/origin/* # timeout=10
            > git rev-parse refs/remotes/origin/main^{commit} # timeout=10
            > git config core.sparsecheckout # timeout=10
            > git checkout -f 4bd5e46bfe80fbc3e4cf0dec8df1f1c033b61c29 # timeout=10
            > git branch -a -v --no-abbrev # timeout=10
            > git checkout -b main 4bd5e46bfe80fbc3e4cf0dec8df1f1c033b61c29 # timeout=10
            [Pipeline] // stage
            [Pipeline] stage
            [Pipeline] { (executing Ansible play book)
            [Pipeline] sh
            + ansible-playbook -i hosts apache-playbook.yaml

            PLAY [playbook for installing ansible] *****************************************

            TASK [Gathering Facts] *********************************************************
            ok: [172.31.8.131]

            TASK [installing apache2] ******************************************************
            changed: [172.31.8.131]

            PLAY RECAP *********************************************************************
            172.31.8.131               : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

            Post stage
            [Pipeline] echo
            ========Playbook executed successfully========
            [Pipeline] }
            [Pipeline] // stage
            [Pipeline] stage
            [Pipeline] { (Declarative: Post Actions)
            [Pipeline] echo
            ========pipeline executed successfully ========
            [Pipeline] }
            [Pipeline] // stage
            [Pipeline] }
            [Pipeline] // withEnv
            [Pipeline] }
            [Pipeline] // node
            [Pipeline] End of Pipeline
            Finished: SUCCESS
          ```
        * if build is successful.
          ![preview](images/040.png)
        * Now check for apache Installatin is successful or not.
        * browse the link to check apache 
          ```
          http://<ansible-node-publicIp>
          ```
          ![preview](images/041.png)
        * Installation is successful.
  ---

