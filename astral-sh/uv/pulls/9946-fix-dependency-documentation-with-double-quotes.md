```yaml
number: 9946
title: "FIX: Dependency documentation with double quotes where required"
type: pull_request
state: merged
author: mattdeform
labels:
  - documentation
  - windows
assignees: []
merged: true
base: main
head: updating-single-quotes-to-double
created_at: 2024-12-16T22:11:39Z
updated_at: 2024-12-16T23:39:09Z
url: https://github.com/astral-sh/uv/pull/9946
synced_at: 2026-01-12T16:09:03Z
```

# FIX: Dependency documentation with double quotes where required

---

_@mattdeform_

## Summary
Documentation steps resulted in errors due to single quotes when adding project dependencies:

``` shell
>uv add 'httpx>0.1.0'

error: Failed to parse: `'httpx`
  Caused by: Expected package name starting with an alphanumeric character, found `'`
'httpx
^
```
``` shell
>uv add 'PyQt5; sys_platform == "windows" 
error: Failed to parse: `'PyQt5;`
  Caused by: Expected package name starting with an alphanumeric character, found `'`
'PyQt5;
^
```

## Testing Steps
- Follow new documentation steps

Tested on:
- [x] Windows




---

_Comment by @zanieb on 2024-12-16 23:20_

What shell are you using here? `cmd.exe` or Powershell? These commands are valid most POSIX shells afaik?

---

_Comment by @mattdeform on 2024-12-16 23:32_

> What shell are you using here? `cmd.exe` or Powershell? These commands are valid most POSIX shells afaik?

Just standard cmd.exe.  I just tested in Powershell, and can confirm the currently documented steps work as expected.

---

_@zanieb approved on 2024-12-16 23:38_

---

_Label `documentation` added by @zanieb on 2024-12-16 23:39_

---

_Label `windows` added by @zanieb on 2024-12-16 23:39_

---

_Merged by @zanieb on 2024-12-16 23:39_

---

_Closed by @zanieb on 2024-12-16 23:39_

---

_Comment by @zanieb on 2024-12-16 23:39_

Thanks!

---
