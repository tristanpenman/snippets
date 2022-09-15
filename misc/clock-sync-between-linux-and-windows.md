# Clock sync between Linux and Windows

## Problem

When dual booting between Linux and Windows, a common problem is that the clock becomes out of sync between the two operating systems. This is because Linux systems are typically configured to store the time on the hardware clock as UTC. Windows, by default, uses local time.

## Answer

[This answer](https://askubuntu.com/a/169384) on Ask Ubuntu helped to explain the problem, and provided several solutions.

It seems that The Right Way to fix this is to make Windows use UTC.

Create a file named WindowsTimeFixUTC.reg with the following contents and then double click on it to merge the contents with the registry:

    Windows Registry Editor Version 5.00
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation]
        "RealTimeIsUniversal"=dword:00000001

Windows Time service will still write local time to the RTC regardless of the registry setting above on shutdown, so it is handy to disable Windows Time service with this command (if time sync is still required while in Windows use any third-party time sync solution):

    sc config w32time start= disabled
