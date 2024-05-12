# learn-rhel-login-as-root
How to login to rhel as root or other users

## How to read logs

```ruby
sudo tail -f /var/log/secure

journalctl _COMM=sshd
```
https://serverfault.com/questions/465833/where-is-the-sshd-log-file-on-red-hat-linux-stored

## Set unconfined users to confined
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/using_selinux/managing-confined-and-unconfined-users_using-selinux#confining-regular-users_managing-confined-and-unconfined-users
