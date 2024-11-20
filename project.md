# Ansible Configuration Management Setup 

![image](https://github.com/user-attachments/assets/f75692ae-ebdc-46e0-ac78-fce8b61416cb)

## Update the Name Tag on Jenkins EC2 Instance

1. Update the Name tag on your Jenkins EC2 Instance to `Jenkins-Ansible`.

   ![image](https://github.com/user-attachments/assets/66e9f175-86b5-4a10-a33e-1a8305847199)


## Install and Configure Ansible on EC2 Instance

### Step 1: Connect to Your EC2 Instance
1. In the AWS Console, go to **EC2** and locate your instance for Jenkins-Ansible.
2. Copy the **Public IP** of the instance.
3. Use SSH to connect to your instance:
   ```bash
   ssh -i path_to_your_key.pem ubuntu@your_instance_public_ip
   ```
  ![image](https://github.com/user-attachments/assets/46bc6fdf-bea0-491c-b268-cb03ee7f0a8e)


4. Once connected, update the instance:
   ```bash
   sudo apt update
   ```
   ![image](https://github.com/user-attachments/assets/c8e34a84-dacd-4e3c-bfdc-e11d121441d2)


### Step 2: Install Ansible
1. Install Ansible directly on the instance:
   ```bash
   sudo apt install ansible -y
   ```
   ![image](https://github.com/user-attachments/assets/93d75893-8c3b-4846-8b39-681e32567d5f)

![image](https://github.com/user-attachments/assets/5b8261ba-5684-402f-9e6b-80d0e48e2ecc)

2. Check the Ansible version to confirm the installation:
   ```bash
   ansible --version
   ```
  ![image](https://github.com/user-attachments/assets/1fb6ca95-1b20-4eb0-855d-034e42e637ac)

---

## Configure Jenkins for Archiving Repository Content

### Step 1: Create a GitHub Repository
1. Go to your GitHub account and create a new repository named `ansible-config-mgt`.

   ![image](https://github.com/user-attachments/assets/9a920ab3-431b-410e-96ed-02cdb7124ac2)


### Step 2: Set Up a New Freestyle Project in Jenkins
1. Open Jenkins and sign in at `http://your_instance_public_ip:8080`.


2. Click **New Item** on the Jenkins dashboard, name the project `ansible`, select **Freestyle project**, and click **OK**.
   ![image](https://github.com/user-attachments/assets/9d2a7ea2-2a73-4e75-bc85-683948ace259)


3. In the **Source Code Management** section, select **Git**.
4. Paste the URL of your `ansible-config-mgt` GitHub repository and add your GitHub credentials.
   ![- **Screenshot**](https://github.com/Prince-Tee/AnsibleConfigMgt/blob/main/screenshot%20from%20my%20local%20env/in%20jekins%20choose%20git%20and%20put%20in%20the%20repository%20url%20and%20your%20github%20credential.PNG)

5. Allocate an **Elastic IP** to the Jenkins-Ansible server to maintain a consistent IP address.

![image](https://github.com/user-attachments/assets/8f710933-c5cf-4d8b-b0fc-09ae2854f4b5)

![image](https://github.com/user-attachments/assets/d8039346-e31b-4406-a61f-a694475ab715)

### Step 3: Set Up a GitHub Webhook to Trigger Jenkins Build
1. In GitHub, go to **Settings** for `ansible-config-mgt` repository.
2. Under **Webhooks**, click **Add webhook**.
3. Set **Payload URL** to `http://your_instance_public_ip:8080/github-webhook/`.
4. Set **Content type** to `application/json`.
5. Choose **Just the push event** to trigger the webhook.
6. Click **Add webhook**.
  ![image](https://github.com/user-attachments/assets/b4b4dff4-7c75-4e03-b709-feab8eda4e06)

---

## Configure Jenkins to Archive Build Artifacts

1. Go back to Jenkins and click **Configure** on the project.
2. Scroll down to **Post-build Actions**.
3. In **Post-build Actions**, select **Archive the artifacts**.
4. In **Files to archive**, type `**` to archive all files.
5. Check **GitHub hook trigger for GITScm polling**.
6. Click **Save** to finish configuring the Jenkins job.
  ![image](https://github.com/user-attachments/assets/ec4b95d1-3a38-4d5b-a97a-6149fea85ba2)

---

## Test the Setup

1. In your `ansible-config-mgt` GitHub repository, edit `README.md` (e.g., add a new line).
2. Commit the changes directly to the master branch.

   ![image](https://github.com/user-attachments/assets/f1817f81-d32a-4419-aa12-b6e286863eeb)


4. In Jenkins, you should see a build triggered automatically.

   ![image](https://github.com/user-attachments/assets/a470e5dc-d9c7-4db7-aa83-2fb1e51eb1bd)

6. After the build completes, verify that the artifacts are archived in:
   ```bash
   ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/
   ```
   Replace `<build_number>` with the specific build number.
 ![image](https://github.com/user-attachments/assets/dc50b977-7f6f-4938-9317-6814cd27c621)


---

## Clone Repository to Jenkins-Ansible EC2 Instance

1. On the EC2 instance terminal, navigate to the desired directory.
2. Clone the `ansible-config-mgt` repository:
   ```bash
   git clone <repository-link>
   ```
   Replace `<repository-link>` with the GitHub repository URL.

   ![image](https://github.com/user-attachments/assets/806ef45f-fc25-4488-8619-fe1ed61153e8)

---

## Prepare Development Environment with Visual Studio Code

1. Download and install **Visual Studio Code** on your local machine.
2. Open Visual Studio Code and configure Git integration:
   - Go to **View > Terminal**.
   - Navigate to the directory where the repository is located.
3. Clone the repository locally:
   ```bash
   git clone <repository-link>
   ```
![image](https://github.com/user-attachments/assets/af4ff3cd-0c9f-4916-95c2-25704061f235)


---

## Set Up Branch Structure

1. Navigate to `ansible-config-mgt` on GitHub.
2. Go to **Branches** and click **New Branch**.
3. Name the branch descriptively, such as `feature/setup-inventory-playbooks`.

![image](https://github.com/user-attachments/assets/8c750834-bd0d-467c-ae38-7b3719f0560b)

![image](https://github.com/user-attachments/assets/6a1a589e-07f6-4875-9077-20e8abc35351)



4. Create and checkout the branch locally:
   ```bash
   git pull origin main
   git checkout -b feature/setup-inventory-playbooks
   ```
  ![image](https://github.com/user-attachments/assets/78e99e81-4a73-47c8-b809-feab67e4f27a)


### Create Directory Structure for Ansible Files
1. Within the repository folder, create the following directories:
   ```bash
   mkdir playbooks
   mkdir inventory
   ```

2. Create an empty playbook file named `common.yml` inside `playbooks`.
   ```bash
   cd playbooks
   touch common.yml
   ```
![image](https://github.com/user-attachments/assets/3d3296ad-07e2-4ada-9284-14aaad229fc3)

---

## Set Up Ansible Inventory

1. In Visual Studio Code, create 4 files in the `inventory` folder named `dev`, `staging`, `uat`, and `prod`.
   
![image](https://github.com/user-attachments/assets/6bf33bc7-638b-44fa-81b2-d1b4a5c54242)

3. Edit the **Development Inventory File (dev)** to define hosts:
   ```ini
   [nfs]
   <NFS-Server-Private-IP-Address> ansible_ssh_user=ec2-user

   [webservers]
   <Web-Server1-Private-IP-Address> ansible_ssh_user=ec2-user
   <Web-Server2-Private-IP-Address> ansible_ssh_user=ec2-user

   [db]
   <Database-Private-IP-Address> ansible_ssh_user=ec2-user 

   [lb]
   <Load-Balancer-Private-IP-Address> ansible_ssh_user=ubuntu
   ```
  ![image](https://github.com/user-attachments/assets/dcda977a-5da8-4e9d-a171-5a9a0854fdbf)

---

## Set Up SSH Agent and Import Private Key

1. Start the SSH Agent:
   ```bash
   eval ssh-agent -s
   ```

2. Navigate to `.ssh` directory and list files to locate the private key.
   ```bash
   cd ~/.ssh
   ls
   ```

3. Add the private key:
   ```bash
   ssh-add ~/.ssh/id_rsa
   ```
![image](https://github.com/user-attachments/assets/1f101dc1-ca6a-4c04-aed1-27b1c2691e1c)



## Update and Push Changes to GitHub

1. Add and commit the changes to Git:
   ```bash
   git add .
   git commit -m "Set up Ansible directory structure and development inventory"
   ```
2. Push changes to the feature branch:
   ```bash
   git push origin feature/setup-inventory-playbooks
   ```


---
## Connect to Jenkins-Ansible EC2 from Visual Studio Code

1. Open Visual Studio Code and install the **Remote - SSH** extension.
  
2. Press `F1`, type `Remote-SSH: Connect to Host`, and enter:
   ```plaintext
   ssh ubuntu@<your-public-ip>
   ```
 ![image](https://github.com/user-attachments/assets/101271a3-49c7-4219-9a7b-e712c5bd66d7)


3. Edit files directly on the server as needed.

## Configure and Test Ansible Playbook

1. Update `playbooks/common.yml` with the following configuration:
   ```yaml
   ---
   - name: update web, nfs, and db servers
     hosts: webservers, nfs, db
     become: yes
     tasks:
       - name: ensure wireshark is at the latest version
         yum:
           name: wireshark
           state: latest

   - name: update LB server
     hosts: lb
     become: yes
     tasks:
       - name: Update apt repo
         apt: 
           update_cache: yes

       - name: ensure wireshark is at the latest version
         apt:
           name: wireshark
           state: latest
   ```
![- **Screenshot**](https://github.com/Prince-Tee/AnsibleConfigMgt/blob/main/screenshot%20from%20my%20local%20env/updating%20the%20common%20yml%20file.PNG)


## Additional Tasks in Ansible Playbook

In common.yml playbook you created in vscode you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.


1. Add the following additional tasks to create a directory, change the timezone, and run a shell script:
   ```yaml
   - name: additional tasks for all servers
     hosts: all
     become: yes
     tasks:
       - name: Create a directory
         file:
           path: /tmp/mydirectory
           state: directory

       - name: Create a file in the new directory
         file:
           path: /tmp/mydirectory/myfile.txt
           state: touch

       - name: Change timezone to UTC
         command: timedatectl set-timezone UTC

       - name: Run a shell script
         shell: /path/to/your/script.sh
   ```
  ![- **Screenshot**](https://github.com/Prince-Tee/AnsibleConfigMgt/blob/main/screenshot%20from%20my%20local%20env/updating%20the%20common%20yml%20file.PNG)


## Update GitHub with Your Code Changes

1. **Check the Status of Git Changes**  
   Run `git status` to see the modified files in your repository:
   ```bash
   git status
   ```

2. **Add Modified or Created Files**  
   Add only the specific files you’ve modified or created (e.g., `common.yml`):
   ```bash
   git add playbooks/common.yml
   ```

3. **Commit the Changes with a Descriptive Message**  
   Provide a clear and descriptive message about the changes made:
   ```bash
   git commit -m "Add common.yml playbook for server updates and configurations"
   ```

4. **Push the Branch to GitHub**  
   If you’re on a feature branch, push your branch to GitHub (e.g., `feature/setup-inventory-playbooks`):
   ```bash
   git push origin feature/setup-inventory-playbooks
   ```

## Create a Pull Request on GitHub

1. On GitHub, go to your repository and create a pull request for your recent changes.
2. Provide a descriptive title and comment about the changes.

![image](https://github.com/user-attachments/assets/6941232e-e7b4-4e17-9878-ddd2d442c214)


![image](https://github.com/user-attachments/assets/7054056a-7365-47f3-995e-f334ef67e5fc)


3. Review and merge the pull request.

![image](https://github.com/user-attachments/assets/889e49e8-2239-495b-a985-25282ef03c0c)


4. Pull the latest changes to your local master branch:
   ```bash
   git checkout main
   git pull origin main
   ```
 ![image](https://github.com/user-attachments/assets/31116385-341f-42b9-81fa-25142403fd20)


## Verify Jenkins Automation (if Integrated with Jenkins)

1. **Jenkins Build Trigger**  
   If Jenkins is set to trigger a job on code changes, it will automatically start a build once your code is merged into the main branch.

![image](https://github.com/user-attachments/assets/e9d9abb3-885f-4a61-949f-7bdc4aa8b579)

2. **Confirm Build Artifacts on Jenkins**  
   - Log into Jenkins and navigate to your project.
   - After the job runs, confirm the build output is saved in `/var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/`.

![image](https://github.com/user-attachments/assets/ff2f92ea-7268-42f8-8390-af5780a0f3be)


## Connect to Your Ansible-Controlled Server from VSCode

1. **Open VSCode**  
   Start VSCode on your local machine.

2. **Connect to Your Remote Instance**  
   - Use the Remote - SSH extension in VSCode.
   - Press `F1`, type `Remote-SSH: Connect to Host...`, and select your server (e.g., `51.21.47.126`).  
   - Choose your operating system (Linux, Windows, or macOS). For this setup, select **Linux**.

   

3. **Handle SSH Permission Errors**  
   If you encounter a "permission denied (publickey)" error, update your SSH config file to include your SSH key:
   ```bash
   IdentityFile "C:\Users\Prince-Tee\Downloads\lampproject.pem"
   ```
   ![*(Screenshot)*](https://github.com/Prince-Tee/AnsibleConfigMgt/blob/main/screenshot%20from%20my%20local%20env/update%20the%20config%20file%20to%20the%20ssh%20key.PNG)

4. **Reconnect to the Host**  
   After updating, press `F1`, type `Remote-SSH: Connect to Host...`, and select the server. Confirm the SSH connection in the terminal and you will see that we can now connect to our server on vscode.

   ![*(Screenshots)*](https://github.com/Prince-Tee/AnsibleConfigMgt/blob/main/screenshot%20from%20my%20local%20env/connecting%20to%20our%20server%20from%20vscode.PNG)
   ![*(Screenshots)*](https://github.com/Prince-Tee/AnsibleConfigMgt/blob/main/screenshot%20from%20my%20local%20env/connection%20to%20our%20server%20on%20vscode.PNG)
   ![*(Screenshots)*](https://github.com/Prince-Tee/AnsibleConfigMgt/blob/main/screenshot%20from%20my%20local%20env/connection%20to%20server%20from%20vscode%20secured.PNG)

---

## Prepare the Ansible Environment

1. **Navigate to Your Ansible Directory**  
   - Once connected to the server through VSCode, open the terminal.
   - Go to your Ansible configuration directory where playbooks and inventory files are located:
     ```bash
     cd ~/ansible-config-mgt
     ```

   ![*(Screenshot)*](https://github.com/Prince-Tee/AnsibleConfigMgt/blob/main/screenshot%20from%20my%20local%20env/Prepare%20the%20Ansible%20Environment.PNG)

2. **Confirm Inventory and Playbook Locations**  
   Check that `inventory/dev.ini` and `playbooks/common.yml` are present:
   ```bash
   ls inventory/dev.ini playbooks/common.yml
   ```
    ![*(Screenshot)*](https://github.com/Prince-Tee/AnsibleConfigMgt/blob/main/screenshot%20from%20my%20local%20env/confirm%20the%20build%20is%20saved%20in%20the%20directory%20var%20jenkins.PNG)

   - If these files are not present, pull them from your repository:
     ```bash
     git pull origin main
     ```

   ![*(Screenshot)*](https://github.com/Prince-Tee/AnsibleConfigMgt/blob/main/screenshot%20from%20my%20local%20env/Prepare%20the%20Ansible%20Environment.PNG)



3. **Run the Ansible Playbook**  
   Run the playbook with the following command:
   ```bash
   ansible-playbook -i inventory/dev.ini playbooks/common.yml
   ```

   ![*(Screenshot)* ](https://github.com/Prince-Tee/AnsibleConfigMgt/blob/main/screenshot%20from%20my%20local%20env/run%20the%20final%20commandncommand.PNG)  


If you encounter the below error
![Screenshot](https://github.com/Prince-Tee/AnsibleConfigMgt/blob/main/screenshot%20from%20my%20local%20env/error%20encoutered.PNG)

It means Ansible is trying to manage a server that uses apt (Ubuntu/Debian package manager) with a task configured for yum (typically used by RHEL/CentOS systems). Since your playbook likely includes a package installation task for wireshark or similar, 
and it’s incorrectly assuming yum instead of apt. so change the content of your dev.ini file to the below to allow the download of the package

```bash
---
- name: update web, nfs, and db servers
  hosts: webservers, nfs, db
  become: yes
  tasks:
    - name: ensure wireshark is at the latest version on RHEL/CentOS servers
      yum:
        name: wireshark
        state: latest
      when: ansible_facts['pkg_mgr'] == "yum"

- name: update LB server
  hosts: lb
  become: yes
  tasks:
    - name: Update apt repository
      apt: 
        update_cache: yes
      when: ansible_facts['pkg_mgr'] == "apt"

    - name: ensure wireshark is at the latest version on Ubuntu/Debian servers
      apt:
        name: wireshark
        state: latest
      when: ansible_facts['pkg_mgr'] == "apt"
```
After that run the playbook again.

![sceenshot](https://github.com/Prince-Tee/AnsibleConfigMgt/blob/main/screenshot%20from%20my%20local%20env/run%20your%20playbook.PNG)

4. **Verify Installation**  
   You can confirm Wireshark installation on each server by running:
   ```bash
   which wireshark
   ```
   or
   ```bash
   wireshark --version
   ```

   ![screenshot](https://github.com/Prince-Tee/AnsibleConfigMgt/blob/main/screenshot%20from%20my%20local%20env/wireshark%20on%20each%20servers.PNG)

 **Conclusion**
 In this Ansible-driven configuration management implementation, we effectively automated infrastructure management using Jenkins and GitHub integration. Key steps included installing and configuring Ansible on the Jenkins-hosted EC2 instance, enabling Jenkins to execute automated builds, manage artifacts, and archive results efficiently. Additionally, a GitHub webhook was set up to trigger builds dynamically upon detecting code changes. This setup also featured structured branch and directory organization within the GitHub repository, the creation of a comprehensive Ansible inventory for resource management, and secure server communication facilitated via SSH key configuration. This cohesive approach enhances configuration management and streamlines infrastructure provisioning, serving as a scalable foundation for advanced automation workflows.
