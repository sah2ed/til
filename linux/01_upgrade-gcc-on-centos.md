# Upgrade gcc 4.4.7 to 4.8 on CentOS 6.7 
On the CentOS box I was trying to install `strongloop`, it shipped with an ancient version of g++ 
which naturally, caused all sorts of compilation [errors](01_strongloop-compilation-errors.txt).

At least, version 4.8 of g++ is required to compile `strongloop` for node.js 4.x.

```sh
wget http://people.centos.org/tru/devtools-2/devtools-2.repo -O /etc/yum.repos.d/devtools-2.repo
yum install devtoolset-2-gcc devtoolset-2-binutils devtoolset-2-gcc-c++
```

Confirm the install succeeded with:
```
/opt/rh/devtoolset-2/root/usr/bin/gcc --version
/opt/rh/devtoolset-2/root/usr/bin/cpp --version
/opt/rh/devtoolset-2/root/usr/bin/c++ --version
```

Each of the previous 3 commands should print output similar to:
```
gcc (GCC) 4.8.2 20140120 (Red Hat 4.8.2-15)
```


Next is to export their unusual location for `node-gyp` to use them when compiling:
```
export CC=/opt/rh/devtoolset-2/root/usr/bin/gcc  
export CPP=/opt/rh/devtoolset-2/root/usr/bin/cpp
export CXX=/opt/rh/devtoolset-2/root/usr/bin/c++
``` 

[1] http://stackoverflow.com/questions/32431894/how-to-install-or-upgrade-g-in-centos-6

[2] http://superuser.com/questions/381160/how-to-install-gcc-4-7-x-4-8-x-on-centos