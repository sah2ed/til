# MongoDB

Several of the basic MongoDB commands which I have used in the past are now escaping me 
so it is time to write them down as snippets so I can easily referenced them.

## Installation on Windows
After running the v3.2.6 Windows [installer](https://fastdl.mongodb.org/win32/mongodb-win32-x86_64-2008plus-ssl-3.2.6-signed.msi), 
the database directory and log file path still need to be configured for MongoDB to start successfully. 
Ha, Just like old times (for ~ v2.x).

### 1.0. Create the `dbpath` and `logpath`
```
mkdir D:\Apps\MongoDB\data
mkdir D:\Apps\MongoDB\log
```

### 2.0. Create a `mongod.cfg` file
```
cat > D:\Apps\MongoDB\mongod.cfg
dbpath=D:\Apps\MongoDB\data
logpath=D:\Apps\MongoDB\log\mongod.log
```


Apparently, the format of the `mongod.cfg` file I used for v2.x still works for v3.x.
```
systemLog:
    destination: file
    logpath: D:\Apps\MongoDB\log\mongod.log
storage:
    dbPath: D:\Apps\MongoDB\data
```


### 3.0. Install `mongod` as a Windows service
```
SET PATH=D:\Apps\MongoDB\Server\3.2\bin;%PATH%
mongod -f D:\Apps\MongoDB\mongod.cfg --install
```
