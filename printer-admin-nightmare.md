## Printers being an admin nightmare (ref CVE-2021-34527)


DO NOT - RestrictDriverInstallationToAdministrators

```
reg add "HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows NT\Printers\PointAndPrint" /v RestrictDriverInstallationToAdministrators /t REG_DWORD /d 0 /f
```

or

DO - RestrictDriverInstallationToAdministrators
```
reg add "HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows NT\Printers\PointAndPrint" /v RestrictDriverInstallationToAdministrators /t REG_DWORD /d 1 /f
```


## Background context and discussions:

https://krebsonsecurity.com/2021/07/microsoft-issues-emergency-patch-for-windows-flaw/

https://www.reddit.com/r/sysadmin/comments/qdeley/print_nightmare_becomes_reality/

https://www.reddit.com/r/sysadmin/comments/slkdsn/anyone_have_a_final_printnightmare_resolution/

https://www.reddit.com/r/sysadmin/comments/pa0lup/kb5005652/

https://www.reddit.com/r/sysadmin/comments/qbb2e0/kb5006670_october_cu_breaks_network_printing/

----

## Possible solutions and post about this:

Extract from TheITBros.com [post](http://theitbros.com/allow-non-admins-install-printer-drivers-via-gpo/)

Microsoft recommends using Group Policy to install printers on users’ computers. But this only works for v4 Package-aware print drivers. When installing any v3 driver, a UAC window appears asking for an administrator password.

If you cannot update all drivers to v4, there is one workaround. On all problem computers, it is necessary to install the parameter RestrictDriverInstallationToAdministrators =0 in the registry key HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint\.

The easiest way to deploy this registry parameter to computers is through Group Policy

Create a new registry parameter under the GPO section Computer Configuration > Preferences > Windows Settings > Registry.

    Action: Replace
    Hive: HKEY_LOCAL_MACHINE
    Key path: Software\Policies\Microsoft\Windows NT\Printers\PointAndPrint
    Value name: RestrictDriverInstallationToAdministrators
    Value type: REG_DWORD
    Value data: 0

group policy install printer driver without admin rights

This registry key will allow users to connect to any printer.

This is a workaround, not a fix, because it makes your print servers vulnerable (!!!!).

Therefore, you additionally need to configure the Point and Print Restriction policy (described above). Add trusted print servers in the “Users can only point and print to these servers” section.

Additionally, set:

    When installing drivers for a new connection: Do not show warning or elevated prompt;
    When upgrading drivers for an existing connection: Show warning only.
    
    

