# Enable a 1GB Swap file on CentOS 6.7 (512MB DigitalOcean droplet)
I kept getting the message "Killed" on the console while a long running compilation of 
several binaries for `strongloop` on a [DigitalOcean (DO)](https://www.digitalocean.com/) droplet with only 512MB of RAM.

The compilation of optional and required dependencies using node-gyp was started with `npm install -g strong-pm`.

Turns out to be throttling put in place by DO to keep developers from abusing the resources of the underlying hardware.

To keep the `node-gyp` compilation from being killed by the DigitalOcean hypervisor, I had to enable a swap file.

## 1.0.
Create and restrict access to the swap file:
```
fallocate -l 1G /swapfile
chmod 600 /swapfile
```

## 2.0.
Confirm the swap file size and permissions:
```
ls -lh /swapfile
-rw------- 1 root root 1.0G May  5 19:00 /swapfile
```

## 3.0.
Format the swap file:
```
mkswap /swapfile
mkswap: /swapfile: warning: don't erase bootbits sectors
        on whole disk. Use -f to force.
Setting up swapspace version 1, size = 1048572 KiB
no label, UUID=e0f42abf-1251-4070-a8d2-b5be045af508
```

## 4.0. 
Enable the swap file:
```
swapon /swapfile
```

## 5.0. 
Confirm the swap file is enabled:
```
swapon -s  # free -m
Filename                                Type            Size    Used    Priority
/swapfile                               file            1048572 0       -1
```


[1] https://www.digitalocean.com/community/questions/npm-gets-killed-no-matter-what?answer=18115

[2] https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04
