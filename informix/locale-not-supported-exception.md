# Locale not supported 

```
java.sql.SQLException: Locale not supported.
```

## TL;DR
The fix was to append the numeric equivalent of UTF8 to the JDBC URL.

So, this didn't work: `DB_LOCALE=en_US.UTF8;CLIENT_LOCALE=en_US.UTF8`
but this worked: `DB_LOCALE=en_US.57372;CLIENT_LOCALE=en_US.57372`



## Problem
Experienced this error again today while attempting to connect to an Informix instance using two different SQL workbench tools that use JDBC.
The JDBC URL was essentially something like: 
```
jdbc:informix-sqli://192.168.1.103:2021/sysmaster:informixserver=informixoltp_tcp
```

To fix the issue, I appended `DB_LOCALE=en_US.UTF8;CLIENT_LOCALE=en_US.UTF8` to the JDBC URL but the error remained.

I tested different variations of the URL parameters using RazorSQL but it appears to be poorly coded 
-- it continually gave the same error even with a non-existent IP address for the Informix server so I had to abandon it.

Tried again with Aqua Data Studio v17.0.3 which gave the same error message but it wasn't as instantenous as RazorSQL.
I'm theorizing that RazorSQL was perhaps parsing the URL parameters to ensure the parameter values were valid 
prior to establishing a network connection to the Informix server. 

Eventually, I realized that `en_US.UTF8 == en_US.57372`, tried the following JDBC URL and it worked!
```
jdbc:informix-sqli://192.168.1.103:2021/sysmaster:informixserver=informixoltp_tcp;DB_LOCALE=en_US.57372;CLIENT_LOCALE=en_US.57372
```


## Prior Research
Additional info which I happened upon when I first ran into the issue.

`java.sql.SQLException locale unsupported DB_LOCALE=en_US.utf-8`

And it turns out that viewing the `NLS Capabilities and Attributes` for one of the (user-defined) databases using `dbaccess` showed that:
```
en_US.57372 Collating Sequence
en_US.57372 CType
```

Baffling since the Windows registry is setup with `DB_LOCALE=en_US.utf-8` for the Informix server and no where else on the system. 


## Random Tidbits
- `glfiles` utility displays the locales available on an Informix server. ([docs](https://www-01.ibm.com/support/knowledgecenter/SSGU8G_12.1.0/com.ibm.glsug.doc/ids_gug_270.htm%23ids_gug_270?lang=en))

- To check the condensed version of the database locale in Informix, based on info gleaned from [[1](https://www-01.ibm.com/support/knowledgecenter/SSGU8G_12.1.0/com.ibm.glsug.doc/ids_gug_037.htm)] and [[2](https://www-01.ibm.com/support/knowledgecenter/SSGU8G_12.1.0/com.ibm.glsug.doc/ids_gug_051.htm?lang=en)]:
```
database sysmaster;
select tabid, tabname, site from systables where tabid = 90; -- 90, "GL_COLLATE", "en_US.819"
select tabid, tabname, site from systables where tabid = 91; -- 91, "GL_CTYPE", "en_US.819"
```


