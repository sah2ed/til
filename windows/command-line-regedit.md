# Command Line Registry Editing

I needed to disable the very annoying taskbar thumbnail preview in Windows 8.1 and happened upon this [answer](http://superuser.com/a/800619/52840).

Instead of opening `regedit`, fiddling with right-clicks et al, a faster method is `reg add` from the command line.

The original answer when rewritten for `reg add` looks like this:
```
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v ExtendedUIHoverTime /t REG_DWORD /d 0xfffffffe 
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Taskband /v NumThumbnails /t REG_DWORD /d 0x00000000
```

Simply execute it from a `Command Prompt` launched as `Administrator` then restart Windows Explorer using the Task Manager.


[1] http://superuser.com/questions/755553/how-to-disable-taskbar-thumbnail-preview-in-windows-8/1075156