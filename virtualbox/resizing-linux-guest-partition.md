# Resizing a Linux guest partition

Needed to increase disk space in VirtualBox Linux VM from `12GB` to `32GB`. Ubuntu is the guest OS.

Done this before but found a clear reminder of the steps in the blog post below.

```
C:\Apps\Oracle\VirtualBox\VBoxManage.exe modifyhd "C:\Users\sah2ed\VirtualBox VMs\Ubuntu-14.04\Ubuntu-14.04-disk1.vdi" --resize 32768
```

Then use GParted to resize the partition to use the free space.


[source](http://logicmason.com/2012/growing-a-hard-drive-partition-in-a-virtualbox-ubuntu-guest/)

