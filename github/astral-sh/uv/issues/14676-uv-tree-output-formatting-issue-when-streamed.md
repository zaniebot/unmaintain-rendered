---
number: 14676
title: uv tree output formatting issue, when streamed into textfile.
type: issue
state: closed
author: 666hwll
labels:
  - question
  - windows
assignees: []
created_at: 2025-07-17T07:21:48Z
updated_at: 2025-08-11T12:42:43Z
url: https://github.com/astral-sh/uv/issues/14676
synced_at: 2026-01-07T13:12:18-06:00
---

# uv tree output formatting issue, when streamed into textfile.

---

_Issue opened by @666hwll on 2025-07-17 07:21_

### Summary

When ```uv tree > dependency_tree.txt``` is being run, it outputs this:
```
projectname v0.1.1
Ôö£ÔöÇÔöÇ matplotlib v3.10.3
Ôöé   Ôö£ÔöÇÔöÇ contourpy v1.3.2
Ôöé   Ôöé   ÔööÔöÇÔöÇ numpy v2.3.1
Ôöé   Ôö£ÔöÇÔöÇ cycler v0.12.1
Ôöé   Ôö£ÔöÇÔöÇ fonttools v4.59.0
Ôöé   Ôö£ÔöÇÔöÇ kiwisolver v1.4.8
Ôöé   Ôö£ÔöÇÔöÇ numpy v2.3.1
Ôöé   Ôö£ÔöÇÔöÇ packaging v25.0
Ôöé   Ôö£ÔöÇÔöÇ pillow v11.3.0
Ôöé   Ôö£ÔöÇÔöÇ pyparsing v3.2.3
Ôöé   ÔööÔöÇÔöÇ python-dateutil v2.9.0.post0
Ôöé       ÔööÔöÇÔöÇ six v1.17.0
ÔööÔöÇÔöÇ pymeasure v0.15.0
    Ôö£ÔöÇÔöÇ numpy v2.3.1
    Ôö£ÔöÇÔöÇ pandas v2.3.1
    Ôöé   Ôö£ÔöÇÔöÇ numpy v2.3.1
    Ôöé   Ôö£ÔöÇÔöÇ python-dateutil v2.9.0.post0 (*)
    Ôöé   Ôö£ÔöÇÔöÇ pytz v2025.2
    Ôöé   ÔööÔöÇÔöÇ tzdata v2025.2
    Ôö£ÔöÇÔöÇ pint v0.24.4
    Ôöé   Ôö£ÔöÇÔöÇ flexcache v0.3
    Ôöé   Ôöé   ÔööÔöÇÔöÇ typing-extensions v4.14.1
    Ôöé   Ôö£ÔöÇÔöÇ flexparser v0.4
    Ôöé   Ôöé   ÔööÔöÇÔöÇ typing-extensions v4.14.1
    Ôöé   Ôö£ÔöÇÔöÇ platformdirs v4.3.8
    Ôöé   ÔööÔöÇÔöÇ typing-extensions v4.14.1
    Ôö£ÔöÇÔöÇ pyqtgraph v0.13.7
    Ôöé   ÔööÔöÇÔöÇ numpy v2.3.1
    Ôö£ÔöÇÔöÇ pyserial v3.5
    ÔööÔöÇÔöÇ pyvisa v1.15.0
        ÔööÔöÇÔöÇ typing-extensions v4.14.1
(*) Package tree already displayed
``` 
Verbose: ``` 
DEBUG Found workspace root: `C:\Work\projectname`
DEBUG Adding root workspace member: `C:\Work\projectname`
DEBUG Reading Python requests from version file at `C:\Work\projectname\.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.13`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: measurement @ file:///C:/Work/projectname
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
```

### Platform

Windows 10 Professional x86-64

### Version

uv 0.7.21 (77c771c7f 2025-07-14)

### Python version

Python 3.13.5

---

_Label `bug` added by @666hwll on 2025-07-17 07:21_

---

_Comment by @zanieb on 2025-07-17 12:52_

It looks like it's failing to render the unicode tree? I'm not sure what we can do about that. How are you viewing the file?

---

_Label `bug` removed by @zanieb on 2025-07-17 12:52_

---

_Label `question` added by @zanieb on 2025-07-17 12:52_

---

_Label `windows` added by @zanieb on 2025-07-17 12:52_

---

_Comment by @666hwll on 2025-07-17 13:09_

Through Neovim and VS Codium. I think a fallback option would be good.
Those fancy chars could be replaced by tabs, just like python works.

---

_Comment by @zanieb on 2025-07-17 13:16_

How would we know we should fall back? Unicode is pretty ubiquitous these days, I'm confused that they're not rendering for you.

---

_Comment by @666hwll on 2025-07-17 13:39_

This issue only occurs when streamed, so a handling scenario for streaming to files would be good for a modern cli-tool.

---

_Closed by @666hwll on 2025-08-11 12:42_

---
