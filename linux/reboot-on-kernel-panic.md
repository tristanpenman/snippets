# Reboot on Kernel panic

It can be useful on embedded/IoT Linux devices to automatically reboot on a kernel panic/oops.

I found some advice on how to do this [in this blog post](https://utcc.utoronto.ca/~cks/space/blog/linux/RebootOnPanicSettings).

In short, there are two sysctls that you can set to achieve this:

    kernel.panic=<seconds-before-reboot>
    kernel.panic_on_oops=<0|1>

These can be set as boot parameters. For example, to reboot after 10 seconds on any kernel panic/oops:

    panic=10 panic_on_oops=1
