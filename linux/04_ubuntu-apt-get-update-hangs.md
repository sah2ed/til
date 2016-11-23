# apt-get update hangs forever

Executed:
```bash
apt-get update && apt-get install docker-engine
```
but the command appeared to hang forever at:
```
Ign http://extras.ubuntu.com trusty/main Translation-en
100% [Connecting to archive.ubuntu.com (2001:67c:1560:8001::11)] [Connecting
```

## Fix
Had to uncomment out `precedence ::ffff:0:0/96 100` in `/etc/gai.conf`.

[1] https://zach-adams.com/2015/01/apt-get-cant-connect-to-security-ubuntu-fix/
