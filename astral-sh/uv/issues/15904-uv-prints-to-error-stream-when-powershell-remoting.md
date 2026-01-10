---
number: 15904
title: uv prints to error stream when powershell remoting
type: issue
state: closed
author: may-day
labels:
  - external
assignees: []
created_at: 2025-09-17T06:41:41Z
updated_at: 2025-09-17T11:55:26Z
url: https://github.com/astral-sh/uv/issues/15904
synced_at: 2026-01-10T01:26:01Z
---

# uv prints to error stream when powershell remoting

---

_Issue opened by @may-day on 2025-09-17 06:41_

### Summary

When calling `uv sync` in a remote powershell output is apparently printed to errorstream resulting in a powershell error message:
```powershell
~\progs\flagit ❯ uv sync
Resolved 2 packages in 1ms
Audited 2 packages in 0.43ms
~\progs\flagit ❯
~\progs\flagit ❯ Enter-PSSession -ComputerName localhost
[localhost]: PS ~\progs\flagit> uv sync
uv : Resolved 2 packages in 1ms
    + CategoryInfo          : NotSpecified: (Resolved 2 packages in 1ms:String) [], RemoteException
    + FullyQualifiedErrorId : NativeCommandError

Audited 2 packages in 0.90ms
[localhost]: PS ~\progs\flagit>
```

Not sure if this is a powershell problem or uv problem.
My powershell version:
```powershell
~ ❯ $PSVersionTable

Name                           Value
----                           -----
PSVersion                      5.1.26100.4768
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.26100.4768
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1

```

### Platform

Windows 11

### Version

uv 0.8.17 (10960bc13 2025-09-10)

### Python version

Python 3.11

---

_Label `bug` added by @may-day on 2025-09-17 06:41_

---

_Comment by @konstin on 2025-09-17 10:25_

CC @Gankra 

---

_Comment by @zanieb on 2025-09-17 10:57_

I think this is intentional and an unfortunate side-effect of how powershell parses output compared to unix tooling.

---

_Comment by @may-day on 2025-09-17 11:25_

@zanieb But only happens in a remote powershell? In a local powershell it doesn't have that effect.

---

_Comment by @konstin on 2025-09-17 11:25_

Are you sure those are the same shells with the same versions?

---

_Comment by @may-day on 2025-09-17 11:30_

@konstin yes it's the same, just double checked.

---

_Comment by @zanieb on 2025-09-17 11:38_

I think this is still "by-design" https://github.com/PowerShell/PowerShell/issues/3996

---

_Label `bug` removed by @zanieb on 2025-09-17 11:39_

---

_Label `external` added by @zanieb on 2025-09-17 11:39_

---

_Comment by @may-day on 2025-09-17 11:55_

@zanieb Indeed. Guess i'll wait then for powershell7 coming preinstalled by default :)

---

_Closed by @may-day on 2025-09-17 11:55_

---
