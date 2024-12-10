# Cloud Engineering Second Semester Examination Project

## (Provision a Linux Server with a simple HTML page)

This a step-by-step documentation of how I provisioned the server, installed the web server, deployed the HTML page, and configured networking.

### 1. Provisioning the Server
On my EC2 dashboard, I clicked on the 'Lauch instance' button to begin the process of provisioning a server instance.

On the Launch an instance page, I labelled the instance as "Cloud Exam Project". I then proceeded to select:
- an Ubuntu AMI (Amazon Machine Image)
- the t2.micro instance type, and 
- my already created key pair (for SSH)

![launch an instance 1](https://github.com/dontcatchfire/cloud-exam-project/blob/main/images/CEP%20-%201.png)

![launch an instance 2](https://github.com/dontcatchfire/cloud-exam-project/blob/main/images/CEP%20-%202.png)

For Network settings, I used the default firewall security groups and allowed inbound traffic on ports 22 (SSH), 80 (HTTP), 443 (HTTPS).

I did a quick double-check to make sure I chose the right configurations, left every other setting as is, and then launched the instance.

The instance was running. 🥳

### 2. Web Server Setup
Using Termius (an SSH client and terminal) on my machine, I navigated to the folder where I had my SSH public key.

I then pasted my SSH access command in the terminal to SSH into the instance.

_(I learned you'd need to be in the folder/directory where your public key is located, so that your local machine and the remote instance can authenticate without any issues)._

Once I was in the server, I ran the `sudo apt update` command to update the Ubuntu OS.

Then I ran the `sudo apt install -y apache2` to install the Apache2 web server.

I copied and pasted my Public IPv4 address to confirm Apache2 was running, and it was affirmative 👍.

### 3. HTML Page Deployment
The HTML page I created for this project is hosted on my GitHub account in the "cloud-exam-project" repo (this repo that you're currently view). 

So, to download the project from the repository unto the server, I ran the commands 
`wget https://github.com/dontcatchfire/cloud-exam-project.git` and `wget https://github.com/dontcatchfire/cloud-exam-project/archive/refs/heads/main.zip`

I unzipped the main.zip file to extract its content. Unzipping the main.zip file created the **cloud-exam-project-main** folder, which  I cd'd into the folder to get to the index.html file.

I then moved the *index.html* file into the */var/www/html/* directory using the `mv index.html /var/www/html/` command 

I revisted my IP address and my index.html file loaded up as the default landing page. 🙌

### 4. Networking
The networking configurations were already set to allow inbound traffic on ports 80 and 443 during the instance provisioning. 

## Deliverables 
* IP Address: 34.253.25.168
* Screenshot showing your HTML page in a browser.
![]()

