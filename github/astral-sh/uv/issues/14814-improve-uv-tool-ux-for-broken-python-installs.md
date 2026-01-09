---
number: 14814
title: Improve uv tool UX for broken Python installs
type: issue
state: open
author: amalachow
labels:
  - enhancement
assignees: []
created_at: 2025-07-22T15:59:44Z
updated_at: 2025-08-02T19:20:23Z
url: https://github.com/astral-sh/uv/issues/14814
synced_at: 2026-01-07T13:12:19-06:00
---

# Improve uv tool UX for broken Python installs

---

_Issue opened by @amalachow on 2025-07-22 15:59_

### Summary

Similar/related to #11534/#12538

The current UX for the `uv tool` sub commands can detect broken links in the tools venv and recommends using `--reinstall` to remediate; however, the recommended command fails due to the same broken link. _e.g._

`uv tool list` yields the following
```
warning: Querying Python at `C:\Users\amalachow\AppData\Roaming\uv\tools\chardet\Scripts\python.exe` failed with exit status exit code: 103

[stderr]
No Python at '"C:\Python312\python.exe'
 (run `uv tool install chardet --reinstall` to reinstall)
```
yet running the recommended command results in near identical output
`uv tool install chardet --reinstall`
```
error: Querying Python at `C:\Users\amalachow\AppData\Roaming\uv\tools\chardet\Scripts\python.exe` failed with exit status exit code: 103

[stderr]
No Python at '"C:\Python312\python.exe'
```

there is a valid workflow by first uninstalling before attempting to run the failed sub command.
* Is it reasonable to swallow the error on cases where the Python interpreter is explicitly being changed with `--python` or some variant/combination of `--force`/`--refresh`/`--reinstall` to improve the UX for these commands?
* Should the suggested command recommend the uninstall + reinstall sequence instead?

### Example

_No response_

---

_Label `enhancement` added by @amalachow on 2025-07-22 15:59_

---

_Referenced in [astral-sh/uv#15032](../../astral-sh/uv/issues/15032.md) on 2025-08-02 19:08_

---

_Comment by @hwine on 2025-08-02 19:18_

FYIW, a scripted way to recover from this in bash is:
```bash
for tool in $(uv tool list 2>&1 | grep install | cut -d\  -f6); do \
    uv tool uninstall $tool && \
    uv tool install $tool \
; done
```

---
