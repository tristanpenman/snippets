# Remove residual Debian packages

Running `dpkg --list` you can see a list of packages on your system, some of which may be marked `rc`, or residual. These may be the result of upgrading or removing packages over time.

This easy one-liner can help you to clean out those residual packages:

    sudo apt remove --purge $(dpkg -l | awk '/^rc/{print $2}')
