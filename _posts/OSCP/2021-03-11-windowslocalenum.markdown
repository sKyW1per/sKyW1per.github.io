---
title: Windows Local Enumeration
author: linatl
date: "2021-03-11 00:01"
tags: [OSCP, Cheatsheet]
layout: post
---

###### PrivEsc Checklists
```
Hacktricks XYZ
https://book.hacktricks.xyz/windows/checklist-windows-privilege-escalation

Sushant747
https://sushant747.gitbooks.io/total-oscp-guide/content/privilege_escalation_windows.html

PayloadsAllTheThings
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md
```


###### System
```
> systeminfo
> systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
> wmic qfe
> wmic qfe get Caption,Description,HotFixID,InstalledOn
> wmic logicaldisk
> wmic logicaldisk get caption,description,providername
> dir /R
> ls env:
```

###### Users
```
> whoami
> whoami /groups
> whoami /priv
(if Both SeChangeNotifyPrivilege and SeImpersonatePrivilege are enabled:
  possible potato attack vuln)  
> net user
> net user Administrator
> net user /domain
> net group
> net localgroup

the ‘> net localgroup’ command only works when you are an actual user, not webroot, www-data or another system account.
But looking for an existing group, like ‘> net localgroup administrators’ does work.
```

###### Network
```
> ipconfig /all
> arp -a
> route print
> netstat -ano
```

###### Password Hunting
```
Looking for strings “password”
> findstr /si password *.txt *.config *.ini
> findstr /si password *.ini
> findstr /si password *.config
> findstr /si password *.txt
> findstr /spin “password” *.*

Find these strings in config files
> dir /s *pass* == *cred* == *vnc* == *.config*

Search a specific file
> dir “\local.txt” /s

Wifi passwords
> netsh wlan show profile
> netsh wlan show profile SSID key=clear

Search for the string 'Password' in the registry
> reg query HKLM /f Password /t Reg_SZ /s
> reg query HKCU /f password /t REG_SZ /s

And more:
https://pentestlab.blog/2017/04/19/stored-credentials/
```

###### AV and Firewall
```
> sc query windefend
> sc queryex type= service
> netsh firewall show config
> netsh advfirewall firewall dump

(for older machines):
> netsh firewall show state
```

###### WinPEAS
```
https://github.com/carlospolop/PEASS-ng
x86
cp /opt/PEASS-ng/winPEAS/winPEASexe/binaries/x64/Release/winPEASx64.exe ./
x64
cp /opt/PEASS-ng/winPEAS/winPEASexe/binaries/x86/Release/winPEASx86.exe ./
```

###### Automated tools ~ PowerUp
```
Instruction how to run sherlock and powerup
http://virgil-cj.blogspot.com/2018/02/escalation-time.html

PowerUp (from PowerSploit)
https://github.com/PowerShellMafia/PowerSploit/tree/master/Privesc
> cp /usr/share/windows-resources/powersploit/Privesc/PowerUp.ps1 ./

On the target machine:
CMD
> powershell -exec bypass -command "& { Import-Module .\PowerUp.ps1; Invoke-AllChecks; }" > output.txt
Or PowerShell
> Import-Module ./PowerUp.ps1
> Invoke-AllChecks | Out-File -Encoding ASCII output.txt
```

###### Automated tools ~ Sherlock and Watson
```
Sherlock
https://github.com/rasta-mouse/Sherlock
Watson
https://github.com/rasta-mouse/Watson

On the target machine:
CMD
> powershell.exe -exec bypass -Command "& {Import-Module .\Sherlock.ps1; Find-AllVulns}" > output.txt
Or PowerShell
> Import-Module ./Sherlock.ps1
> Find-AllVulns | Out-File -Encoding ASCII output.txt
```

###### Windows Exploit Suggester - Next Generation
```
https://github.com/bitsadmin/wesng
Getting the info for wes:
> systeminfo > systeminfo.txt
> python3 /opt/wesng/wes.py --update
> python3 /opt/wesng/wes.py --definitions=/opt/wesng/definitions.zip -o vulns.txt systeminfo.txt
> python3 /opt/wesng/wes.py --definitions=/opt/wesng/definitions.zip --exploits-only --impact "Elevation of Privilege" --hide "Internet Explorer" Edge Flash -o vulns.txt systeminfo.txt
> python3 /opt/wesng/wes.py --exploits-only --impact "Elevation of Privilege" --definitions=/opt/wesng/definitions.zip  systeminfo.txt

> cat vulns.txt | cut -d "," -f 2,4,9 | uniq | sort

```

###### Metasploit Exploit Suggester
```
Metasploit Local Exploit Suggester
https://blog.rapid7.com/2015/08/11/metasploit-local-exploit-suggester-do-less-get-more/
```