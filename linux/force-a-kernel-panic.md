# Force a kernel panic

Found on [IBM Support](https://www.ibm.com/support/pages/forcing-fake-kernel-panic-testing)

## Question

How do I force a fake kernel panic?

## Answer

For testing purposes, you can force a kernel panic. This tool is typically used for testing kernel panic traps or validating cluster fail over during a hard lock up.

To force a kernel panic, echo a trigger into /proc:

    # echo 1 > /proc/sys/kernel/sysrq
    # echo "c" > /proc/sysrq-trigger
