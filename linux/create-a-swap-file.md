# Create a swap file

Based on documentation found [here](https://phoenixnap.com/kb/linux-swap-file).

## Check for existing swap files

The easiest way to check is using `free -h`. The output will look something like this:

```
               total        used        free      shared  buff/cache   available
Mem:           1.9Gi       648Mi       271Mi       8.6Mi       1.2Gi       1.3Gi
Swap:          3.8Gi        55Mi       3.8Gi
```

The `Swap` line will only be present if swap is active.


Alternatively, you can query it from the `meminfo` file in procfs:
```
cat /proc/meminfo | grep Swap
```

This will list various swap statistics:
```
SwapCached:        14560 kB
SwapTotal:       3999996 kB
SwapFree:        3942848 kB
```

To see _where_ the swap file is located, you can query `/proc/swaps`:
```
cat /proc/swaps
```
or use `swapon` if it is installed:
```
swapon --show
```

e.g.
```
$ cat /proc/swaps
Filename				Type		Size		Used		Priority
/swapfile                               file		3999996		57148		-2

$ swapon --show
NAME      TYPE SIZE  USED PRIO
/swapfile file 3.8G 55.8M   -2
```

## Create a new swap file

Allocate space for the swap file:
```
sudo dd if=/dev/zero of=/swapfile bs=1MB count=1024
```

Fix permissions:
```
sudo chmod 600 /swapfile
```

Use `mkswap` to set up a properly formatted swap area in the space allocated:
```
sudo mkswap /swapfile
```

Finally, the new swap file can be activated:
```
sudo swapon /swapfile
```

## Mount on startup

To activate the swap file on startup, you will need to update `/etc/fstab`:
```
# Add this:
/swapfile swap swap defaults 0 0
```
