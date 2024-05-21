# learn-rhel-login-as-root
How to login to rhel as root or other users

When use RHEL it will create /etc/ssh/sshd_config.d/50-cloud-init.conf and in it this will get append to.
```ruby
PasswordAuthentication no
```

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
Check if /etc/ssh/sshd_config.d/50-cloud-init.conf or /etc/ssh/sshd_config.d/50-redhat.conf and disable accordingly

You just need to enable
```ruby
PermitRootLogin yes
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
rm -rf /etc/ssh/sshd_config.d/50-cloud-init.conf.bak
mv /etc/ssh/sshd_config.d/50-cloud-init.conf /etc/ssh/sshd_config.d/50-cloud-init.conf.bak
# Restart sshd to apply changes
systemctl restart sshd

# Check architecture
ARCH=$(uname -m)

if [ "$ARCH" == "x86_64" ]; then
    # Install for 64-bit (x86_64) architecture
    dnf install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
elif [ "$ARCH" == "aarch64" ]; then
    # Install for ARM architecture (arm64)
    dnf install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_arm64/amazon-ssm-agent.rpm
else
    echo "Unsupported architecture: $ARCH"
    exit 1
fi

sudo systemctl enable amazon-ssm-agent
sudo systemctl start amazon-ssm-agent
systemctl status amazon-ssm-agent
```
Resource - https://phoenixnap.com/kb/ssh-permission-denied-publickey
## Set unconfined users to confined
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/using_selinux/managing-confined-and-unconfined-users_using-selinux#confining-regular-users_managing-confined-and-unconfined-users
