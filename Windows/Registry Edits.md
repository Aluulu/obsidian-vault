
Disabling fast boot:
```cmd
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Power" /v HiberbootEnabled /t reg_dword /d 0 /f
```

Turning on num lock at boot:
```cmd
Set-ItemProperty -Path 'Registry::HKU.DEFAULT\Control Panel\Keyboard' -Name "InitialKeyboardIndicators" -Value "2"
```
