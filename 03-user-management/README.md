# User Management in Linux

## Introduction to User Management in Linux
Linux is a multi-user operating system, meaning multiple users can operate on a system simultaneously. Proper user management ensures security, controlled access, and system integrity. 

Key files involved in user management:
- `/etc/passwd` – Stores user account details.
- `/etc/shadow` – Stores encrypted user passwords.
- `/etc/group` – Stores group information.
- `/etc/gshadow` – Stores secure group details.

## Creating Users in Linux
To create a new user in Linux, use:

### `useradd` Command (For most Linux distributions)
```bash
useradd username
```
This creates a user without a home directory.

To create a user with a home directory:
```bash
useradd -m username
```

To specify a shell:
```bash
useradd -s /bin/bash username
```

### `adduser` Command (For Debian-based systems)
```bash
adduser username
```
This is an interactive command that asks for a password and additional details.

### Interview Question: Q1. Diff b/w useradd and adduser.
A. 'adduser' creates a user with a home directory and takes a lot of user information.
'useradd' is a quick way of creating users which doesn't ask for any other details and doesnt create any home directory. It is useful for writting shell scripts where we don't want the command to prompt inputs.

### Q2. Can we decript/restore the password of a linux user ?
A. No, because the password has been created with strong hashing and cannot be decripted by any way.
 

## Managing User Passwords
To set or change a user’s password:
```bash
passwd username
```

### To switch to another user
```bash
su - username
```

### Enforcing Password Policies
- **Password expiration**: Set password expiry days
  ```bash
  chage -M 90 username
  ```
- **Lock a user account**
  ```bash
  passwd -l username
  ```
- **Unlock a user account**
  ```bash
  passwd -u username
  ```

## Modifying Users
Modify an existing user with `usermod`:
- Change the username:
  ```bash
  usermod -l new_username old_username
  ```
- Change the home directory:
  ```bash
  usermod -d /new/home/directory -m username
  ```
- Change the default shell:
  ```bash
  usermod -s /bin/zsh username
  ```

## Deleting Users
To remove a user but keep their home directory:
```bash
userdel username
```
To remove a user and their home directory:
```bash
userdel -r username
```

## Working with Groups
### Creating Groups
```bash
groupadd groupname
```

### Adding Users to Groups
```bash
usermod -aG groupname username
```

### Viewing Group Memberships
```bash
groups username
```

### Changing Primary Group
```bash
usermod -g new_primary_group username
```

## Sudo Access and Privilege Escalation
### Adding a User to Sudo Group
On Debian-based systems:
```bash
usermod -aG sudo username
```
On RHEL-based systems:
```bash
usermod -aG wheel username
```

### Granting Specific Commands with Sudo
Edit the sudoers file:
```bash
visudo
```
Then add:
```bash
username ALL=(ALL) NOPASSWD: /path/to/command
```

### To login to a Remote Server using SSH
Open an SSH Client such as gitbash, Putty or terminal etc.
```bash
ssh username@ip_adress
```
```bash
enter password
```
Some organisations allow login for users without entering password by-default. This is done by defining 'PasswordAuthentication' as No in the below file location.
```bash
cat etc/ssh/sshd_config.d/60-cloudimg-settings.config
```
We can we even make it to yes to enable entering password for users to login, and then we can apply the changes using:
```bash
sudo systemctl restart ssh
```
If PasswordAuthentication is disabled, we need to login using .pem file.

### To verify the Linux Instance
```bash
uname -a
```
