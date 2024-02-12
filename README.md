# Linux-Assignment
Linux Assignment


#  1) SMTP Configuration for Localhost
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
