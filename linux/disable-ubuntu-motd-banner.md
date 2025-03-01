# Disable Ubuntu MOTD banner

https://www.hydrogen18.com/blog/disabling-ubuntu-motd-ssh-banner.html

Edit /etc/pam.d/login, to comment out these lines:
```
# Prints the message of the day upon successful login.
# (Replaces the `MOTD_FILE' option in login.defs)
# This includes a dynamically generated part from /run/motd.dynamic
# and a static (admin-editable) part from /etc/motd.
session    optional   pam_motd.so motd=/run/motd.dynamic
session    optional   pam_motd.so noupdate
```

Also comment out the same lines in /etc/pam.d/sshd:
```
session    optional     pam_motd.so  motd=/run/motd.dynamic
session    optional     pam_motd.so noupdate
```

Now run `sudo systemctl restart ssh`.

Done!
