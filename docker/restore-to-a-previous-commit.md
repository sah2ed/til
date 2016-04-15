# Restore to a previous commit

Needed to restore to a previous version of a container I had executed `docker commit` against some hours ago.

`docker history sah2ed/informix:latest`
```
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
daaf9da84313        2 hours ago         /bin/bash                                       414.8 MB            
2394db3261e6        2 days ago          bash                                            414.7 MB            
<missing>           2 days ago          bash                                            0 B                 
<missing>           2 days ago          bash                                            414.7 MB            
<missing>           2 days ago          bash                                            427 MB              
<missing>           2 weeks ago         /bin/sh -c /home/informix/start-informix.sh     414.7 MB            
```

`docker tag 2394db3261e6 sah2ed/informix:dev`

`docker images`
```
REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
sah2ed/informix          latest              daaf9da84313        2 hours ago         5.457 GB
sah2ed/informix          dev                 2394db3261e6        2 days ago          5.042 GB
```



[source](https://gist.github.com/dasgoll/476ecc7a057ac885f0be)