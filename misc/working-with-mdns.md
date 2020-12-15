

To find Chromecasts that are advertised on the local network, on Mac:

    dns-sd -B _googlecast._tcp

On Linux, we can use `avahi-browse`:

    avahi-browse _googlecast._tcp