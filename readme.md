# Cloud Engineering Second Semester Examination Project

> **Quick Note:** If for whatever reason the images aren't displaying, please try a different ISP. At first, the images weren't showing up when I was on Airtel. But when I switched to MTN, the images then displayed. 
---

## (Provision a Linux Server with a simple HTML page)

This a step-by-step documentation of how I provisioned the server, installed the web server, deployed the HTML page, and configured networking.

### 1. Provisioning the Server
On my EC2 dashboard, I clicked on the 'Lauch instance' button to begin the process of provisioning a server instance.

On the Launch an instance page, I labelled the instance as "Cloud Exam Project". I then proceeded to select:
- an Ubuntu AMI (Amazon Machine Image)
- the t2.micro instance type, and 
- my already created key pair (for SSH)

![labelling the instance](/images/CEP%20-%201.png)

![selecting instance type](images/CEP%20-%202.png)

For Network settings, I used the default firewall security groups and allowed inbound traffic on ports 22 (SSH), 80 (HTTP), 443 (HTTPS).

![network settings](images/CEP%20-%203.png)

I did a quick double-check to make sure I chose the right configurations, left every other setting as is, and then launched the instance.

![instance is running](images/CEP%20-%204.png)
The instance was running. 🥳

### 2. Web Server Setup
Using Termius (an SSH client and terminal) on my machine, I navigated to the folder where I had my SSH public key.

I then pasted my SSH access command in the terminal to SSH into the instance.

![SSHing into server](images/CEP%20-%206.png)

_(I learned you'd need to be in the folder/directory where your public key is located, so that your local machine and the remote instance can authenticate without any issues)._

Once I was in the server, I ran the `sudo apt update` command to update the Ubuntu OS.

Then I ran the `sudo apt install -y apache2` to install the Apache2 web server.

![installing apache2](images/CEP%20-%20installing%20apache2.png)

I copied and pasted my Public IPv4 address to confirm Apache2 was running, and it was affirmative 👍.

![apache2 is running](images/CEP%20-%20apache2%20is%20running.png)

### 3. HTML Page Deployment
The HTML page I created for this project is hosted on my GitHub account in the "cloud-exam-project" repo (this repo that you're currently view). 

![html file on local machine](images/CEP%20-%208.png)

![github repo](images/CEP%20-%209.png)

So, to download the project from the repository unto the server, I ran the commands 
`wget https://github.com/dontcatchfire/cloud-exam-project.git` and `wget https://github.com/dontcatchfire/cloud-exam-project/archive/refs/heads/main.zip`

![wget commands](images/CEP%20-%20wget%20commands.png)

I unzipped the main.zip file to extract its content. Unzipping the main.zip file created the **cloud-exam-project-main** folder, which  I cd'd into the folder to get to the index.html file.

![unzipped main.zip](images/CEP%20-%2010.png)

I then moved the *index.html* file into the */var/www/html/* directory using the `mv index.html /var/www/html/` command 

![moved index.html to /var/www/html/](images/CEP%20-%2012.png)

I revisted my IP address and my index.html file loaded up as the default landing page. 🙌

### 4. Networking
The networking configurations were already set to allow inbound traffic on ports 80 and 443 during the instance provisioning. 

## Deliverables 
* IP Address: 34.253.25.168
* Domain name: [dontcatchfire.com](https://dontcatchfire.com)
* Screenshot showing my HTML page in a browser.

![my html page in a browser](/images/my%20html%20page%20in%20a%20browser.png "html page without https and domain")
* html page with with HTTPS 

![my secure html page in a browser](images/secure%20html%20page.png "html page with https and domain")

## Bonus Tasks (Optional for Extra Credit)

> Configure HTTPS for your web server using a free SSL certificate (e.g., Let’s Encrypt).

### Stage 1 - linking domain name to IP address

To configure HTTPS on my Apache server, I first had to route a domain name to my instance's IPv4 address.

To do that, I used Route 53, which is a DNS on AWS.

On Route 53 page, I selected the *Create hosted zones* option to get started.

![Getting started with hosted zone](images/creating%20hosted%20zone%201.png "Getting started with hosted zone")

On the next page, 
- I typed in the domain name I'd be using to route to the server's IPv4 address (dontcatchfire.com), 
- I selected the *public hosted zone* type to make it accessible by anyone over the Internet, and 
- finally clicked the "Create hosted zone" button. 

![Creating hosted zone](/images/creating%20hosted%20zone%202.png "Creating hosted zone")

When the hosted zone for the domain name was created, it came with **NS** and **SOA** records by default. But I needed **A** and **CNAME** records for the routing. So, I clicked on the "Create record" button to create them.

![Hosted zone created](images/hosted%20zone%201.png "Hosted zone created")

On the "Create record" page... 
- for the *Record name* field, I left it empty (because A records are usually reserved for root domains) 
- for the *Record type* field, I selected the A record option 
- and then in the *Value* field, I pasted in the server's IPv4 address
- I left every other setting as they were and clicked on the "Create record" button.

![Creating A record](images/hosted%20zone%202.png "Creating A record")

The A record was successfully created. I then clicked the "Create record" button again to create the CNAME record. 

![A record successfully created](images/hosted%20zone%203.png "A record successfully created")

On the "Create record" page... 
- for the *Record name* field, I put in "www" to function as a sub domain.
- for the *Record type* field, I selected the CNAME record option 
- and then in the *Value* field, I pasted in the root domain name
- Every other setting was as they were. I clicked on "Create record" button to complete the whole process.

![Creating CNAME record](images/hosted%20zone%204.png "Creating CNAME record")

The CNAME record was successfully created.

![CNAME record successfully created](images/hosted%20zone%205.png "CNAME record successfully created")

### Stage 2 - Getting SSL certificate from Let's Encrypt

To get the SSL certificate from Let's Encrypt, I ran the command 

`sudo apt install certbot python3-certbot-apache -y`  (to install Certbot and the required Apache plugin), 

followed by `sudo certbot --apache` (which runs Certbot to automatically configure SSL for my domain). 

I followed the prompts by registering with my email address and choosing the domain I wanted to secure.

![Configuring SSL cert on apache](images/finis%20-%20configuring%20ssl%201.png "Configuring SSL cert on apache")

![Configuring SSL cert on apache - completed](images/finis%20-%20configuring%20ssl%202.png "Configuring SSL cert on apache - completed")

And that's all folks. 

Configuration completed and I've got SSL on my domain name now.

Finis! 🏁