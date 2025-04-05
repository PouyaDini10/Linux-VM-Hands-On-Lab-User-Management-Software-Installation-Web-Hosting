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














