# Project Title

## Table of Contents
1. [Introduction](#introduction)
2. [Objective](#objective)
3. [Requirements](#requirements)
4. [Design](#design)
5. [Implementation](#implementation)
6. [Testing](#testing)
7. [Conclusion](#conclusion)
8. [References](#references)

## Introduction
We need you to implement public key authentication for a few boxes in our environment. I need Security-Desk and Workstation-Desk setup to use public key authentication for ssh connections into the Prod-Web server. Once you have that taken care of we need you to install a one time password package called OTPW and set it up on the Backup server.

## Objective
Increase the security of some of the systems. 

## Requirements
- Create SSH Key for playone (Security Desk, Workstation Desk)
- Create SSH Key for Root (Security Desk)
- Install SSH Key on Production WEB root user
- Implemenmt SSH onetime passwords (Backup)

## Design
| Name | IP Address |
| ------- | ------- |
| Production Web | 172.16.10.7 |
| Backup | 172.16.30.79 |
| Security Desk | 172.16.30.6 |
| Workstation Desk | 172.16.30.101 |

## Implementation
### Create SSH Keys
 1. `ssh-keygen`
### Copy SSH Keys
#### Linux
1. `ssh-copy-id user@host`
#### Windows
1. Copy id_rsa
2. `ssh user@host`
3. `nano .ssh/authorized_keys`
4. Paste ssh key
### Install OTPW
1. `sudo apt install otpw-bin`
2. `sudo apt install libpam-otpw`
3. `sudo nano /etc/pam.d/sshd`
4. Comment this line out `#@include common-auth`
5. Add the following lines
6. `auth     required     pam_otpw.so`
7. `session  optional     pam_otpw.so`
8. `sudo nano /etc/ssh/sshd_config`
9. Math the following values
10. `UsePrivilegeSeparation yes` 
11. `PubkeyAuthentication yes` 
12. `ChallengeResponseAuthentication yes` 
13. `PasswordAuthentication no` 
14. `UsePAM yes` 
15. `sudo systemctl restart sshd`
16. `cd ~`
17. `otpw-gen >filename.txt`
## Testing
Narrowing it down to steps the first step was the work on the Security desk. I created SSh Keys with `ssh-keygen` on both the playerone user and the root user. I then transfered the keys to the production web server using `ssh-copy-id root@172.16.10.6`. This created the necessary authroized hosts file in the `.ssh` folder. I then verified that these keys worked by attempting to ssh into the machine. It was successful because there was no prompt for a password.  

The next step was to switch to the Workstation Desk. The first thing I did was create the ssh keys in powershell using `ssh-keygen`. Being more familiar with Linux, I attempted to transfer the keys over to the server with the same `ssh-copy-id` command. This command does not work in powershell. I then decided to manually add the public key to the authorized hosts file that was already created. I downloaded PUTTY as it makes it easier to attach ssh keys. After login in i edited the file in the root users `.ssh` folder to include the newly generated ssh key. I used `nano` as my text editor of choice. After restarting ssh on the production web server with `sudo systemctl restart sshd`. Then I ssh back into the production web server but with the attached ssh key. When it did not ask for a password, I knew everything was setup correctly.  

--=

The final step was to setup OTPW on the backup server with the root user. With help from Digital Ocean (Linked Below), I was able to get this up an running in a few minutes. There were two packages that I installed for this, otpw and libpam-otpw using the commands `sudo apt install otpw-bin` and `sudo apt install libpam-otpw`.  Once these were instsalled I then needed to make a change to the SSH PAM configuration file.  

`sudo nano /etc/pam.d/sshd` 
**Comment out the following line** 
`@include common-aith` 
**Add the Following 2 lines** 
`auth     required     pam_otpw.so` 
`session  optional     pam_otpw.so` 

Then edit the SSHD Configuration 
`sudo nano /etc/ssh/sshd_config` 
**Ensure the valued below match** 
`UsePrivilegeSeparation yes` 
`PubkeyAuthentication yes` 
`ChallengeResponseAuthentication yes` 
`PasswordAuthentication no` 
`UsePAM yes` 
**Restart SSH**
`sudo systemctl restart sshd`

**Create OTW Files** 
`cd ~` 
`otpw-gen > filename.txt`



## Conclusion
By establishing secure SSH keys on Security Desk, Workstation Desk, and the Production Web server, we have significantly enhanced the security posture of these machines. The use of OTPW on the Backup server adds an extra layer of protection by requiring one-time passwords for authentication. This mitigates risks associated with compromised credentials and strengthens overall system resilience. 

## References
    - https://www.digitalocean.com/community/tutorials/install-and-use-otpw
