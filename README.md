action uses wow64 redirection false

// === 1. Kill any running Skype for Business processes ===
waithidden taskkill /IM lync.exe /F

// === 2. Remove auto-start registry values for current user ===
waithidden reg delete "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v "Skype for Business" /f
waithidden reg delete "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v "Lync" /f
waithidden reg delete "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v "Lync99" /f

// === 3. Remove auto-start registry values for all users (if running as SYSTEM) ===
waithidden reg delete "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v "Skype for Business" /f
waithidden reg delete "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v "Lync" /f
waithidden reg delete "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v "Lync99" /f

// === 4. Remove desktop shortcut for all users and current user ===
delete __appendfile
appendfile @echo off
appendfile del "%PUBLIC%\Desktop\Skype for Business.lnk" /f
appendfile del "%PUBLIC%\Desktop\Lync.lnk" /f
appendfile del "%USERPROFILE%\Desktop\Skype for Business.lnk" /f
appendfile del "%USERPROFILE%\Desktop\Lync.lnk" /f
move __appendfile delete_shortcuts.bat
waithidden cmd.exe /c delete_shortcuts.bat

// === 5. Unpin Skype from taskbar for current user ===
// Note: This uses PowerShell and works best on Windows 10/11 with modern profiles
delete __appendfile
appendfile $app = "Skype for Business"
appendfile $path = (New-Object -ComObject Shell.Application).NameSpace('Shell:Taskbar').Items() | Where-Object { $_.Name -eq $app }
appendfile if ($path) { $path.Verbs() | Where-Object { $_.Name.replace('&','') -match 'Unpin from taskbar' } | ForEach-Object { $_.DoIt() } }
move __appendfile unpin.ps1
waithidden powershell -ExecutionPolicy Bypass -File unpin.ps1
