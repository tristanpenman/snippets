# Prevent Apps Loading on Startup

This annoying feature is controlled by a single file in each user account.

First we need to flag the file as owned by root (otherwise macOS will replace it) with the following command:

    sudo chown root ~/Library/Preferences/ByHost/com.apple.loginwindow.*

Next we need to remove all permissions (so that it can not be read or written to) with the following command:

    sudo chmod 000 ~/Library/Preferences/ByHost/com.apple.loginwindow.*
