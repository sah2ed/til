# Connect to Informix running inside Docker container running inside VirtualBox

Needed to connect to an Informix server running inside a docker container. 
The docker container itself was being run inside a VirtualBox Ubuntu guest OS.


## Guest OS
1. Start the Informix docker container and the Informix server.
```
docker run -it --name informix -p 2021:2021 <namespace>/informix:<tag> /bin/bash
oninit -vy
```

2. Confirm that container port forwarding for the Informix server has been established.
```
sudo netstat -pant
```
```
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.1.1:53            0.0.0.0:*               LISTEN      1114/dnsmasq    
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      2333/cupsd      
tcp6       0      0 ::1:631                 :::*                    LISTEN      2333/cupsd      
tcp6       0      0 :::2021                 :::*                    LISTEN      2992/docker-proxy
tcp6       1      0 ::1:43651               ::1:631                 CLOSE_WAIT  996/cups-browsed
```

3. Confirm that the Informix server is accessible from *outside* the Informix container (but within the guest OS).
3.1. Get the Informix container's IP address.
```
docker inspect informix | grep IPAddress
```
```
            "SecondaryIPAddresses": null,
            "IPAddress": "172.17.0.2",
                    "IPAddress": "172.17.0.2",

```
3.2. Test the connection.
```
telnet 172.17.0.2 2021
```

4. Obtain the IP address of the guest OS.
```
ifconfig eth0
```
```
eth0      Link encap:Ethernet  HWaddr 08:00:27:c0:1b:0d  
          inet addr:192.168.1.103  Bcast:192.168.1.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fec0:1b0d/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:4510 errors:0 dropped:0 overruns:0 frame:0
          TX packets:2130 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:5744995 (5.7 MB)  TX bytes:176012 (176.0 KB)
```



## Host OS
1. Change the networking settings for the guest OS in VirtualBox to `Bridged Adapter`.

2. Use the IP address obtained (step #4) to confirm that the Informix server is accessible from the host OS.
```
telnet 192.168.1.103 2021
```


[source](http://stackoverflow.com/a/33814957/177696)

