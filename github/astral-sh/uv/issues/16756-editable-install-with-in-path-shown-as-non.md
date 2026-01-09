---
number: 16756
title: "Editable install with @ in path shown as non-editable with uv pip list (ambiguous user/pass - file:***@2) [0.9.9 Regression]"
type: issue
state: closed
author: matthuisman
labels:
  - bug
assignees: []
created_at: 2025-11-17T03:44:02Z
updated_at: 2025-11-17T20:02:29Z
url: https://github.com/astral-sh/uv/issues/16756
synced_at: 2026-01-07T13:12:19-06:00
---

# Editable install with @ in path shown as non-editable with uv pip list (ambiguous user/pass - file:***@2) [0.9.9 Regression]

---

_Issue opened by @matthuisman on 2025-11-17 03:44_

### Summary

This issue started occurring in uv 0.9.9

Installing an editable package located at filepath
`C:/jenkins/workspace/ython_Environment_Manager_PR-251@2/tmp/python_environments_znid30ea/venv%201/workspace/pyproject`

then
`uv pip list --verbose`

```
WARN Failed to parse direct URL: ambiguous user/pass authority in URL (not percent-encoded?): file:***@2/tmp/python_environments_znid30ea/venv%201/workspace/pyproject

Package            Version
------------------ -------
importlib-metadata 8.7.0
pip                25.3
pyproject          1.0.0
```

site-packages\pyproject-1.0.0.dist-info\direct_url.json contents
`{"url":"file:///C:/jenkins/workspace/ython_Environment_Manager_PR-251@2/tmp/python_environments_znid30ea/venv%201/workspace/pyproject","dir_info":{"editable":true}}
`

### Platform

Windows 100

### Version

uv 0.9.9 (4fac4cb7e 2025-11-12)

### Python version

Python 3.10.11

---

_Label `bug` added by @matthuisman on 2025-11-17 03:44_

---

_Comment by @matthuisman on 2025-11-17 03:46_

if I manually edit the direct_url.json and remove the @, it then shows as editable

---

_Comment by @matthuisman on 2025-11-17 03:47_

Suspect caused by https://github.com/astral-sh/uv/pull/16622

---

_Referenced in [astral-sh/uv#16622](../../astral-sh/uv/pulls/16622.md) on 2025-11-17 03:47_

---

_Renamed from "Editable install with @ in path shown as non-editable with uv pip list (ambiguous user/pas) [0.9.9]" to "Editable install with @ in path shown as non-editable with uv pip list (ambiguous user/pass) [0.9.9]" by @matthuisman on 2025-11-17 03:51_

---

_Comment by @matthuisman on 2025-11-17 03:52_

I think that code to detect ambiguous user/pass authority in URL might need to first check its a url and not a filepath :)


https://github.com/astral-sh/uv/pull/16622/files#diff-aab70d99b8510cf11ff6865c60615ebe2a2b2d755316427c9893849b59163eefR63
Seems the code looks for : followed by @ 

---

_Renamed from "Editable install with @ in path shown as non-editable with uv pip list (ambiguous user/pass) [0.9.9]" to "Editable install with @ in path shown as non-editable with uv pip list (ambiguous user/pass - file:***@2) [0.9.9]" by @matthuisman on 2025-11-17 03:58_

---

_Renamed from "Editable install with @ in path shown as non-editable with uv pip list (ambiguous user/pass - file:***@2) [0.9.9]" to "Editable install with @ in path shown as non-editable with uv pip list (ambiguous user/pass - file:***@2) [0.9.9 Regression]" by @matthuisman on 2025-11-17 04:35_

---

_Comment by @woodruffw on 2025-11-17 04:41_

Thanks for the report @matthuisman!

Yeah, the URL change you've linked is almost certainly the root cause here, although I'm kind of surprised we're parsing these paths as URLs. I can triage this more tomorrow, but an immediate fix would probably be to only perform the check on URLs with schemes that actually have credentials, i.e. not `file:`.

---

_Comment by @woodruffw on 2025-11-17 04:43_

Oh, I misread: it's not that we parse the path itself as a URL, but that we reencode it as one in that JSON file. That makes sense, and the fix of checking the scheme would handle that.

---

_Comment by @matthuisman on 2025-11-17 04:44_

Sounds great :)

I suspect in the real world, not many users use @ in their dirnames, but its pretty common with Jenkins 

---

_Comment by @woodruffw on 2025-11-17 04:48_

Yeah, I was kind of surprised by that -- that pathname is coming from Jenkins? Do you happen to know if they document that anywhere? It'd be good to read more about, if only for my own education/awareness 🙂

---

_Comment by @matthuisman on 2025-11-17 08:50_

@woodruffw I could only find this:
https://www.jenkins.io/doc/book/managing/system-properties/#hudson-slaves-workspacelist

Basically, if your job kicks of parallel steps or there are multiple builds of the job - it creates a unique workspace. 
The default is using @

---

_Referenced in [astral-sh/uv#16759](../../astral-sh/uv/pulls/16759.md) on 2025-11-17 12:38_

---

_Closed by @konstin on 2025-11-17 14:16_

---

_Comment by @matthuisman on 2025-11-17 20:02_

confirmed fixed. Thanks for the fast turn around :)

---
