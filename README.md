# Linux-Assignment
Linux Assignment


#  Q1) SMTP Configuration for Localhost
We can setup the SMTP server using Postfix
## 1: Update Package List

update the package list to ensure you have the latest information:

```bash
sudo apt-get update   # For Debian/Ubuntu
```

## 2: Install Postfix and Mailutils

```bash
sudo apt-get install postfix mailutils   # For Debian/Ubuntu
```
The first part of the Postfix installation is a text-base user interface. Use you keyboard to select 	Internet Site for the type of mail configuration.
On the next screen, enter the domain name of your server. Please note that this doesn’t have any impact on your ability to send mail from Gmail, so if you don’t have a domain name, that’s okay.


## 3: Configure Postfix

Configuration file that is located at /etc/postfix/main.cf

```bash

sudo nano /etc/postfix/main.cf
```

Add the following lines to the main.cf file:


```bash


relayhost = [smtp.gmail.com]:587
myhostname = your_hostname

# Location of sasl_passwd we saved
smtp_sasl_password_maps = hash:/etc/postfix/sasl/sasl_passwd

# Enables SASL authentication for postfix
smtp_sasl_auth_enable = yes
smtp_tls_security_level = encrypt

# Disallow methods that allow anonymous authentication
smtp_sasl_security_options = noanonymous
```

## 4: Configure Simple Authentication and Security Layer (SASL) 

Create a the file /etc/postfix/sasl/sasl_passwd and add your Gmail address and app password to it like this:
```bash
[smtp.gmail.com]:587 mailid@example.com:qodjkozoaqdmyjjj
```
```bash
sudo postmap /etc/postfix/sasl/sasl_passwd
```


## 5: Use this following commands to Start/Stop Postfix

To start Postfix,

```bash

sudo systemctl start postfix
```
To restart Postfix,

```bash

sudo systemctl restart postfix
```
To stop Postfix

```bash

sudo systemctl stop postfix
```
Ensure that Postfix is enabled to start at boot:

```bash

sudo systemctl enable postfix
```
## 6: Send a Test Email

```bash
sendmail mailid@example.com
To: mailid@example.com
Subject: Test mail #1
This is just a test email
. 
```
#  Q2) Create a user in your localhost, which should not be able to execute the sudo command.
## 1.  Use the adduser or useradd command to create a new user.
```bash
sudo adduser <username>
```
For example,

```bash
sudo adduser prassana
```
## 2.Give the information prompted in the terminal

## 3.So to see to which groups the user belongs to use the following command
```bash
groups <username>
```
## 4.If it belongs to the sudo group use the following command to remove the user from the sudo group 
```bash
sudo deluser <username> sudo
```
## 5 To verify user does not have sudo privileges you can try to run any command with sudo for example
```bash
sudo ls
```

# Q3    Configure your system in such a way that when a user type and executes a describe command from anywhere of the system it must list all the files and folders of the user's current directory.
# Ex:- $ describe
# $  content1 content2
# Content3 content 4

## We can do this by making alias for ls -lt  or ls as follows
## 1. Open your .bashrc file using a text editor. You can do this by running:
```bash
nano ~/.bashrc
```
## 2. Add the following line at the end of the file:
```bash
alias describe="ls"
```
## 3. Save the file and exit the text editor.
## 4. To apply the changes, either restart your terminal or run:
```bash
source ~/.bashrc
```
## Now, when you type describe in your terminal, it will behave the same as ls.
