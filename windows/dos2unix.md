# Recursively remove carriage return in text files to be deployed on Linux

Windows unhelpfully appends the carriage return `\r` character before the line feed `\n` in the EOF marker for files.
This was the ultimate cause of the `bash: ./start_jupyter: Permission denied` and `Process exited with status 126` errors I was seeing on Heroku.


Using PowerShell, the `dos2unix` fix was to execute:
```
Get-ChildItem -File -Recurse | % { $x = get-content -raw -path $_.fullname; $x -replace "`r`n","`n" | set-content -path $_.fullname }
```

After fixing up the text files, I needed to set the executable flag on the file by doing 
`web: chmod +x ./start_jupyter && ./start_jupyter` inside the `Procfile`.



Source https://github.com/PowerShell/Win32-OpenSSH/wiki/Dos2Unix---Text-file-format-converters
