# Linux-Assignment
Linux Assignment


#  Q1) SMTP Configuration for Localhost
We can setup the SMTP server using Postfix
### 1. Update Package List

update the package list to ensure you have the latest information:

```bash
sudo apt-get update   # For Debian/Ubuntu
```

### 2 Install Postfix and Mailutils

```bash
sudo apt-get install postfix mailutils   # For Debian/Ubuntu
```
The first part of the Postfix installation is a text-base user interface. Use you keyboard to select 	Internet Site for the type of mail configuration.
On the next screen, enter the domain name of your server. Please note that this doesn’t have any impact on your ability to send mail from Gmail, so if you don’t have a domain name, that’s okay.


### 3. Configure Postfix

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

### 4. Configure Simple Authentication and Security Layer (SASL) 

Create a the file /etc/postfix/sasl/sasl_passwd and add your Gmail address and app password to it like this:
```bash
[smtp.gmail.com]:587 mailid@example.com:qodjkozoaqdmyjjj
```
```bash
sudo postmap /etc/postfix/sasl/sasl_passwd
```


### 5. Use this following commands to Start/Stop Postfix

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
### 1.  Use the adduser or useradd command to create a new user.
```bash
sudo adduser <username>
```
For example,

```bash
sudo adduser prassana
```
### 2. Give the information prompted in the terminal

### 3. So to see to which groups the user belongs to use the following command
```bash
groups <username>
```
### 4. If it belongs to the sudo group use the following command to remove the user from the sudo group 
```bash
sudo deluser <username> sudo
```
### 5. To verify user does not have sudo privileges you can try to run any command with sudo for example
```bash
sudo ls
```

# Q3) Configure your system in such a way that when a user type and executes a describe command from anywhere of the system it must list all the files and folders of the user's current directory.
## Ex:- $ describe
## $  content1 content2 Content3 content 4

### We can do this by making alias for ls -lt  or ls as follows
### 1. Open your .bashrc file using a text editor. You can do this by running:
```bash
nano ~/.bashrc
```
### 2. Add the following line at the end of the file:
```bash
alias describe="ls"
```
### 3. Save the file and exit the text editor.
### 4. To apply the changes, either restart your terminal or run:
```bash
source ~/.bashrc
```
### Now, when you type describe in your terminal, it will behave the same as ls.

# Q4)U sers can put a compressed file at any path of the linux file system. The name of the file will be research and the extension will be of compression type, example for gzip type extension will be .gz. You have to find the file and check the compression type and uncompress it. 
## We can write a script for this as follows:

### 1. Find the research file with any extension
```bash
research_file=$(find / -name "research.*" 2>/dev/null)

if [ -z "$research_file" ]; then
echo "Research file not found."
exit 1
fi
```
### 2. Determine compression type
```bash
	compression_type="${research_file##*.}"

	case "$compression_type" in
    gz)
        echo "Found gzip compressed file: $research_file"
        gzip -d "$research_file"
        ;;
    bz2)
        echo "Found bzip2 compressed file: $research_file"
        bzip2 -d "$research_file"
        ;;
    zip)
        echo "Found zip compressed file: $research_file"
        unzip "$research_file"
        ;;
    *)
        echo "Unknown compression type: $compression_type"
        exit 1
        ;;
	esac

	echo "File uncompressed successfully."
```
### 3. Now just run the script file and it will find the file and uncompress it.

# Q5) Configure your system in such a way that any user of your system creates a file then there should not be permission to do any activity in that file.
## Note:- Don’t use the chmod command.
### 1. To restrict all permissions for others, set the umask value to 0777 into your shell file .bashrc

```bash
umask 077
```
### 2. This will ensure that when any user creates a file, the default permissions for others will be restricted, and they won't have any access to the file.

### 3. Save the changes run the following command to apply the new umask setting immediately:

```bash
source ~/.bashrc
```
### 4. For example, if a user creates a file using the touch command:

```bash
touch newfile.txt
```
### 5. The permissions for myfile.txt will be such that only the file owner will have read and write access, while all others will have no permissions. The file owner can still modify its permissions if needed.


# Q6) Create a service with the name showtime , after starting the service, every minute it should print the current time in a file in the user home directory.

## Ex:-
## sudo service showtime start   -> It should start writing in file.
## sudo service showtime stop   -> It should stop writing in file.
## sudo service showtime status -> It should show status.

### To create a systemd service named "showtime" that prints the current time to a file in the user's home directory every minute, follow these steps:
### 1.  Create a Bash script that prints the current time and saves it to a file.

```bash
#!/bin/bash
while true; do
    date | awk '{print $4}'  >> "$HOME/showtime.txt"
    sleep 60 
done
```
### 2. Save this script and make it executable:

```bash
chmod +x /home/<username>/showtime.sh
```
### 3. create a new file as /etc/systemd/system/showtime.service, and add the following content to it:

```bash
[Unit]
Description=Showtime Service

[Service]
Type=simple
ExecStart=/home/showtime.sh
User=<username>
Restart=always

[Install]
WantedBy=multi-user.target
```
### 4. Reload the systemd manager configuration to update the available services:

```bash
sudo systemctl daemon-reload

```
### 5. Enable the "showtime" service to start on boot:

```bash
sudo systemctl enable showtime.service

```
### 6. Start the "showtime" service:

```bash
    sudo systemctl start showtime.service
```
### The "showtime" service is now running and will print the current time to the showtime.txt file in the user's home directory every minute. You can check the output in home directory with in the file called showtime.txt.
