# learn-rhel-login-as-root
How to login to rhel as root or other users

## How to read logs

```ruby
sudo tail -f /var/log/secure

journalctl _COMM=sshd
```

https://serverfault.com/questions/465833/where-is-the-sshd-log-file-on-red-hat-linux-stored
