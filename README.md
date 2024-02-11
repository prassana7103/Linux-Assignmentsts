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
