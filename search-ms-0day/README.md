### Microsoft protocol nightmare (search-ms:)

As mentioned here in this link (and elsewhere)

https://www.bleepingcomputer.com/news/security/new-windows-search-zero-day-added-to-microsoft-protocol-nightmare/

----

In testing we have this registry entry below, which appears to block the 0day. That is due to it disabling "WebSearch".

The 0day quoted, relies on a web call to whatever URL in the first place.

From the BleepingComputer example:

```
search-ms:query=proc&crumb=location:%5C%5Clive.sysinternals.com&displayname=Searching%20Sysinternals
```

With the registry key in place, we could not execute ```search-ms:``` at all via the Windows Start menu utilizing 'search'. 

*search-ms is natively not recognized also in the likes of the Command Line nor Powershell (including Powershell ISE).


### Registry Entry (added to disable Cortana prior to this).

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Windows Search]
"DisableWebSearch"=dword:00000001
"AllowCortana"=dword:00000000
```

----

Sometime back before this 0day was even noted, my clients had all cortana, websearch, bing, telemetry and the likes; all either at a state of what we could say was 'none/disabled' (do not use it) or minimal, if the latter and absolutely required in certain enterprise's.
