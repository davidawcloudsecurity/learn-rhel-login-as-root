# learn-rhel-login-as-root
How to login to rhel as root or other users

## How to read logs

```ruby
sudo tail -f /var/log/secure

journalctl _COMM=sshd
```
https://serverfault.com/questions/465833/where-is-the-sshd-log-file-on-red-hat-linux-stored

## How to enable root/user login using password
```ruby
sudo vi /etc/ssh/sshd_config.d/50-cloud-init.conf

PasswordAuthentication yes
```
```ruby
#!/bin/bash

# Set passwords
USER_PASSWORD="123"
ROOT_PASSWORD="Letmein2021"

# Create user user2
useradd user2
echo "$USER_PASSWORD" | passwd --stdin user2

# Create user admin2 and add to wheel group
useradd -G wheel admin2
echo "$USER_PASSWORD" | passwd --stdin admin2

# Enable root login and set root password
echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config
# Update root password
echo "root:$ROOT_PASSWORD" | chpasswd

# Restart sshd to apply changes
systemctl restart sshd
```

## Set unconfined users to confined
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/using_selinux/managing-confined-and-unconfined-users_using-selinux#confining-regular-users_managing-confined-and-unconfined-users
