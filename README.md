# Linux-VM-Hands-On-Lab-User-Management-Software-Installation-Web-Hosting

## Objective

Goal : Setup a Linux VM(Ubuntu) and I will perform basic tasks like adding users and installing software. 

## Linux Virtual Machine Setup

- Created a resource group called “Linux-VM’s”
- Created a Linux virtual machine(Ubuntu Server 22.04 LTS, 1 cpu would be enough for this lab)
- Give a username and password & create virtual machine

## Manually assign an IP address to the Linux VM

Upon creating this Linux virtual machine, I quickly realized that it wasn't assigned a Public IP address. I believe the issue was azure related such as Cost and Security. We will have to manually assign an IP address. 

To begin I will:
- Find the Networking tab on the left hand side
- Locate the option to select Network Interface
- Find ipconfig1 at the very bottom and click it
- Choose Associate and save

## Connect to the Linux VM

- We will connect to the Linux virtual machine via SSH(Secure Shell) through the powershell
- Open Powershell and enter this prompt: ssh username@[VM’s Public IP Address]
- Enter the password and login(don't be alarmed if you don't see the password being entered, it's still being entered)


Ideally  this is what you want to see:

<img width="407" alt="image" src="https://github.com/user-attachments/assets/b79b9324-938e-4eaa-a272-86c8b9faf6b2" />

## Update the Linux VM

- Input this so the machine checks for any updates: sudo apt update
- Input this after so it actually installs the updates: sudo apt upgrade -y

**Sudo apt update**

<img width="466" alt="image" src="https://github.com/user-attachments/assets/61f575cb-6ab6-477f-9423-4ccd67363dc8" />

As you can see, our Ubuntu system was able to successfully acquire the required updated software packages and all we need to do now is just install it. 

**Sudo apt upgrade -y**

<img width="465" alt="image" src="https://github.com/user-attachments/assets/4eb76b12-0a76-436d-9ec9-a70fd19c04be" />

Your screen may look a little different than mine, but my Ubuntu system has already installed the requirements. You want to be mindful of the bottom portion where it tells you if all the data packages have been successfully installed.

## Linux Tasks

- Create users using this command: sudo adduser testuser
- Install software: Apache Web Server
- Start & enable Apache
- View your web server

### Creating Users

To create a user on our Ubuntu system, the command prompt is “**sudo adduser testuser**”. 

**sudo** = You're telling the computer, “Do this as an admin user”. You need special permissions to create users

**adduser** = The command that creates the user account

**testuser** = The name of the user I’ll be creating(you can pick a name of your choosing).

**All the information inputed are all for demo purposes.**

<img width="467" alt="image" src="https://github.com/user-attachments/assets/8eccd996-2539-403a-affb-11875782219e" />

The command prompt successfully created a new user and it also:
- Created a User Group(category of belonging, helps with User Permissions) for them
- Made a home directory where their personal files will go.
- Copied some default setup files into that home folder.

Simple command to locate testuser : id testuser

Output:

<img width="461" alt="image" src="https://github.com/user-attachments/assets/2b0d7993-cf74-4366-9a09-69d926c6d409" />

## Install Apache(Web Server)

Apache is a program that will allow us to host and show websites. 

Run these Command prompts:

- **sudo apt update** (again just checking for any updates) 

- **sudo apt install apache2 -y** (runs as an admin & will attempt to install Apache & will say yes to every prompt)

<img width="466" alt="image" src="https://github.com/user-attachments/assets/d78a2269-33af-454f-8297-0cf7084f1342" />

My system already has Apache 2, but yours may read differently. 

## Start & Enable Apache

Start Apache 2:

- **sudo systemctl start apache2** ← This command prompt, “systemctl” manages services like Apache.

- **sudo systemctl enable apache2** ← This will simply enable Apache services, even on reboots.

Run a status update to ensure everything is working properly:

- **sudo systemctl status apache2**

 <img width="466" alt="image" src="https://github.com/user-attachments/assets/3601e723-f53e-441c-9a46-4ff86b05f0d5" />

I see that I'm active, now I'm going to attempt to access the web page. 

We're using the public IP address because we are outside of Azure’s internal network. To access the web page we created through our Linux VM, we will need to access it from the internet. A public IP address would alleviate this.

## Troubleshooting Apache Web Server Access on Azure VM

Initially, when I tried to run “http://[your-public-ip]” on my google search bar, the site did not load. I can only assume 2 scenarios:
- Either the web server is not running(which we already confirmed)
- Azure blocks web traffic by default

### Next Step: Allow HTTP(Port 80) in the Azure Network Security Group(NSG)

**HTTP(HyperText Transfer Protocol) Protocol** - It’s the foundation of data communication on the World Wide Web. When you visit a website, your browser and the website’s server talk to each other using HTTP.

**Port 80** - Whenever your browser makes an HTTP request, it's connecting to the server via Port 80. A Port in basic terms can be like a door for your computer or server. Your computers have many ports(0 - 65535) and can handle many different types of network traffic. However, Port 80 is used for non-sensitive websites and if you want to go the secured route, HTTPS(Port 443) is the better encrypted route. 

HTTP(Port 80) uses TCP instead of UDP:

**TCP(Transmission Control Protocol)** - Reliable, ordered, and connection based network protocol that runs a checklist before sending out the network packets.

**UDP (User Database Protocol)** - This protocol is faster, it's unreliable and there’s no guarantee that all the data will be transmitted. 

HTTP(Hyper Text Transfer Protocol) needs a connection focused and reliable data transmission technique. You wouldn't want a half loaded web page. 

**Allow HTTP Port 80 in the Network Security Group(NSG)**

Open Port 80 for HTTP access:
- In azure, locate the network settings for our Linux VM
- Under Inbound Port Rules, create a new port rule
- Use the following settings:
  - Source: Any
  - Source port ranges: *
  - Destination: Any
  - Destination port ranges: 80
  - Protocol: TCP
  - Action: Allow
  - Priority: (e.g., 1000 — lower means higher priority)
  - Name: Allow-HTTP

Now attempt to access the web page, “http://your-Linux-VM's-public-ip”, you should see this: 

<img width="460" alt="image" src="https://github.com/user-attachments/assets/61914280-395d-47c5-b07b-35d9311913b4" />

## Design the Web Page(Through our SSH connection)

Go to the folder where the web files exist: 

Type in the command prompt: **cd /var/www/html**

**cd** - Mean Change directory, it's used to move around your folders structure in the terminal. 

### Remove the default index.html 

If you want to start off with a fresh HTML page, you would have to input this command: **sudo rm index.html**

### Create your own HTML file

You can use a terminal based text editor like nano: **sudo nano index.html**

<img width="362" alt="image" src="https://github.com/user-attachments/assets/55a28757-1309-4c29-bb84-d6b2d4804fe2" />

From here you can begin designing the web page however you'd like. 

### Save and exit

CTRL + O → (Enter) → CTRL + X



























