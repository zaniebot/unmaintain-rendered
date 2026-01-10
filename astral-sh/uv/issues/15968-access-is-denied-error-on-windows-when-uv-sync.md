---
number: 15968
title: "`Access is denied` error on windows when `uv sync`"
type: issue
state: open
author: petesmc
labels:
  - bug
  - windows
  - needs-mre
assignees: []
created_at: 2025-09-21T09:15:00Z
updated_at: 2025-11-21T14:22:10Z
url: https://github.com/astral-sh/uv/issues/15968
synced_at: 2026-01-10T01:26:01Z
---

# `Access is denied` error on windows when `uv sync`

---

_Issue opened by @petesmc on 2025-09-21 09:15_

### Summary

Hi,

Within a corporate environment, when running `uv sync` or `uv add`, I often get an `Access is denied. (OS error 5)` when it is trying to delete a directory. E.g. "Failed to remove directory C:\mylib\.venv\Lib\site-packages\polars-123.**data**". It is usually the **data** or **__pycache__**

I'm unable to replicate this outside of a corporate environment, and cannot provide logs due to company policy. To resolve it I need to run `uv sync` / `uv add` multiple times, depending on the number of dependencies/transistive dependencies, can be 50+ times.

The trace shows the error coming from `remove_dir_all`

I don't see this problem with other package managers like pnpm / cargo, etc.


Potential solution could be using this package which helps with lock issues on Windows: https://crates.io/crates/remove_dir_all



### Platform

Windows 11 x86_64

### Version

0.8.19

### Python version

3.13.5

---

_Label `bug` added by @petesmc on 2025-09-21 09:15_

---

_Label `windows` added by @konstin on 2025-09-23 12:01_

---

_Label `needs-mre` added by @konstin on 2025-09-23 12:01_

---

_Comment by @retropc on 2025-10-31 16:18_

I've seen this too

uv add ruff ->
```
error: Failed to install ruff-0.14.1-py-none-win_amd64.wsl (ruff==0.14.1)
  Caused by: failed to remove directory `C:\.......\.venv\Lib\site-packages\ruff-0.14.1.data`: The filename, directory name, or volume label syntax is incorrect. (os error 123)
```

most likely defender is holding the directory open, need to wait and retry a few times

terrible but that's Windows for you

---

_Referenced in [astral-sh/uv#7382](../../astral-sh/uv/issues/7382.md) on 2025-11-10 11:25_

---

_Referenced in [astral-sh/uv#16747](../../astral-sh/uv/issues/16747.md) on 2025-11-15 17:03_

---

_Comment by @Ndnes on 2025-11-21 14:08_

Not sure if this is the same exact issue, but I am also having problems with Windows Defender. I can not run uv at all though, and I had the same issue with pip as well. This is definitely something local IT / Windows should deal with, but I'll provide the log from Event Viewer anyways.

The event was triggered when running the command "uv --version" in command prompt.


```
<Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
<System>
<Provider Name="Microsoft-Windows-Windows Defender" Guid="{11cd958a-c507-4ef3-b3f2-5fd9dfbd2c78}" /> 
<EventID>1121</EventID> 
<Version>0</Version> 
<Level>3</Level> 
<Task>0</Task> 
<Opcode>0</Opcode> 
<Keywords>0x8000000000000000</Keywords> 
<TimeCreated SystemTime="2025-11-21T12:54:16.9075113Z" /> 
<EventRecordID>1688</EventRecordID> 
<Correlation ActivityID="{4c4fd6cc-21e3-43e3-9c9e-cfc6f13f4fdd}" /> 
<Execution ProcessID="19856" ThreadID="24948" /> 
<Channel>Microsoft-Windows-Windows Defender/Operational</Channel> 
<Computer>***</Computer> 
<Security UserID="S-1-5-18" /> 
</System>
<EventData>
<Data Name="Product Name">Microsoft Defender Antivirus</Data> 
<Data Name="Product Version">4.18.25100.9008</Data> 
<Data Name="Unused" /> 
<Data Name="ID">01443614-CD74-433A-B99E-2ECDC07BFC25</Data> 
<Data Name="Detection Time">2025-11-21T12:54:16.906Z</Data> 
<Data Name="User">***</Data> 
<Data Name="Path">C:\Users\***\.local\bin\uv.exe</Data> 
<Data Name="Process Name">C:\Windows\System32\cmd.exe</Data> 
<Data Name="Security intelligence Version">1.441.381.0</Data> 
<Data Name="Engine Version">1.1.25100.9002</Data> 
<Data Name="RuleType">ENT\ConsR</Data> 
<Data Name="Target Commandline" /> 
<Data Name="Parent Commandline">"C:\WINDOWS\system32\cmd.exe"</Data> 
<Data Name="Involved File" /> 
<Data Name="Inhertiance Flags">0x00000000</Data> 
</EventData>
</Event>
```
 

---
