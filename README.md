action uses wow64 redirection false

waithidden taskkill /IM lync.exe /F

waithidden reg delete "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v "Skype for Business" /f
waithidden reg delete "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v "Lync" /f
waithidden reg delete "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v "Lync99" /f

waithidden reg delete "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v "Skype for Business" /f
waithidden reg delete "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v "Lync" /f
waithidden reg delete "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v "Lync99" /f

parameter "userdesktop" = "{pathname of folder "Desktop" of folder (value of variable "USERPROFILE" of environment)}"
delete __appendfile
appendfile @echo off
appendfile del "%PUBLIC%\Desktop\Skype for Business.lnk" /f
appendfile del "%PUBLIC%\Desktop\Lync.lnk" /f
appendfile del "{parameter "userdesktop"}\Skype for Business.lnk" /f
appendfile del "{parameter "userdesktop"}\Lync.lnk" /f
move __appendfile delete_shortcuts.bat
waithidden cmd.exe /c delete_shortcuts.bat
