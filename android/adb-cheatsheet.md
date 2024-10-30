# ADB Cheatsheet

A list of useful ADB commands, many of which were found [here](https://www.lambdatest.com/blog/adb-commands/).

This isn't a _complete_ list, but just those commands that I find myself looking up from time to time. More commands will be added in time, and contributions are welcome.

## Basics

### `adb connect <ip | id>`

Connect to a specific device.

### `adb shell`

Open a command line shell.

### `adb devices`

List all devices currently connected via ADB.

When connected to multiple devices, you can usually specific which device to perform a command on with the `-s <ip | id>` option. For example:

* `adb -s 192.168.1.10 <cmd...>`

### `adb logcat [options]`

Stream logs from a connect device.

Common options include:

* `adb logcat -c` - Clears the entire log and exits.
* `adb logcat -d` - Dumps the log to the screen and exits.
* `adb logcat -b <buffer>` -	Loads an alternate log buffer for viewing, such as event or radio. The main buffer is used by default.
* `adb logcat -f <path>` - Writes log message output to `<path>`.

### `adb shell ps`

Displays the currently running processes on the device.

## Package Management

### `adb install <apk>`

Install an APK on the device.

### `adb uninstall <pkg>`

Remove a specific package. Note that this takes the package name `<pkg>` rather than a path.

### `adb shell pm list packages [options]`

Lists all packages installed on the device.

Options can include:

* `adb shell pm list packages -d` - List all disabled packages.
* `adb shell pm list packages -e` - List all enabled packages.

### `adb shell pm clear <pkg>`

Clears data storage and cache for a given package, where the `<pkg>` is the package name of the app you want to clear.

### `adb shell dumpsys package <pkg>`

Dump general package information relating to a specific package.

### `adb shell pm disable-user <pkg>`

Disable a package. Good for disabling system packages, which cannot be uninstalled.

You may need to specify a user ID to make this work:

* `adb shell pm disable-user --user 0 <pkg>` - Disable package for user 0 (the default)

### `adb shell pm enable <pkg>`

Enable a package that was previously disabled.

## Activity Management

### `adb shell am start -n <pkg/activity>`

Start a specific activity. For example:

* `adb shell am start -n com.mycompany.mypackage/.MainActivity` - The first `.` after the slash is shorthand for the package prefix.

### `adb shell monkey -p your.app.package.name -c android.intent.category.LAUNCHER 1`

Start a package using `monkey`, pretending to a be a launcher app.

### `adb shell am start -a android.intent.action.VIEW -d <data>`

Start an activity using an intent. Example:

* `adb shell am start -a android.intent.action.VIEW -d http://www.stackoverflow.com`

### `adb shell dumpsys activity`

Dump information relating to the currently running activities (i.e Android Activities).

### `adb shell dumpsys activity broadcasts`

Dump information about the broadcast receivers on the device.

### `adb shell am force-stop <pkg>`

Force activities and services associated with a given package to stop running.

## System Info

### `adb shell dumpsys [option]`

Dump a wide range of system information. Omit `[option]` for a general overview, or use one of the following for more detailed information:

* `battery` - Current battery level, voltage, and temperature.
* `connectivity` - Network connectivity, including the current network type and connection status.
* `cpuinfo` - Detailed CPU usage statistics.
* `display` - Detailed information about the device’s display, including the current display mode and resolution.
* `input` - Current state of all input devices and input queues.
* `netstats` - Network usage statistics.
* `power` - Current battery level, power sources, and wake lock statistics.
* `surface` - Information about currently visible surfaces on your device.
* `window` - Detailed information about the device’s window manager, including the current focus and layout of all windows.

### `adb shell dumpsys meminfo -a <pkg>`

Dump memory usage information for a specific package.

### `adb shell dumpsys gfxinfo <pkg>`

Dump graphics performance statistics for a specific package.

### `adb shell dumpsys input <device-id>`

Dump detailed information about a specific input device.

## Screen Capture

For general screen capture, use [scrcpy](https://github.com/Genymobile/scrcpy).

Alternatively, you can use screen capture features built in to ADB.

### `adb shell screencap <path>`

Takes a screenshot and saves it to a specific `<path>` _on your computer_.

### `adb shell screenrecord <path>`

Records the screen and saves it to a specific `<path>` _on your computer_.

## Power

### `adb reboot [target]`

The reboot command accepts an optional `<target>`, which causes the device to reboot into various modes. Examples include:

* `adb reboot recovery`
* `adb reboot update`

## Serial

Interesting things you can do if you have serial access to a device (e.g. via a USB-to-serial cable).

### `stop adbd` and `start adbd`

Stop or start the ADB daemon running on the device.

## A/B Updates

If you are working with a device that supports A/B partitioning, you can switch boot slots using `bootctl`:

* `adb shell bootctl get-current-slot` - returns 0 or 1 to indicate boot slot
* `adb shell bootctl set-active-boot-slot <0|1>` - enter 0 or 1 to choose which boot slot you want to use

After doing this, run `adb reboot` to reboot into the chosen partition.
