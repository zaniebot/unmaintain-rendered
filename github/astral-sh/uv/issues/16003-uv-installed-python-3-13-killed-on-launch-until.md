---
number: 16003
title: uv-installed Python 3.13 killed on launch until manually codesigned
type: issue
state: open
author: ouatu-ro
labels:
  - bug
assignees: []
created_at: 2025-09-23T15:23:49Z
updated_at: 2025-09-23T16:03:08Z
url: https://github.com/astral-sh/uv/issues/16003
synced_at: 2026-01-07T13:12:19-06:00
---

# uv-installed Python 3.13 killed on launch until manually codesigned

---

_Issue opened by @ouatu-ro on 2025-09-23 15:23_

### Summary

On macOS (Apple Silicon, Sequoia), a `uv python install`ed interpreter is killed immediately on launch due to missing code signature.
Ad-hoc signing fixes it, but it's not obvious to users.

**Steps to reproduce**


```bash
uv python install 3.13

PY313="$HOME/.local/share/uv/python/cpython-3.13.2-macos-aarch64-none/bin/python3.13"
"$PY313" -V
```

or 

```bash
uv init
uv venv 
uv sync
source .venv/bin/activate
python
```
 
Result:

```
[1]    49240 killed     "$PY313" -V
```

Diagnostics:

```bash
spctl -a -vv "$PY313"
/Users/[my_user]/.local/share/uv/python/cpython-3.13.2-macos-aarch64-none/bin/python3.13: rejected

xattr -p com.apple.quarantine "$PY313"
# => no quarantine
```

Workaround:

```bash
codesign --force -s - "$PY313"
"$PY313" -V
# => Python 3.13.2
```

**Expected behavior**

`uv python install` should produce an interpreter that runs out of the box, without requiring the user to manually `codesign`.

**Actual behavior**

Interpreter is killed immediately by macOS AMFI/ExecPolicy until manually signed.

**Additional context**

* `spctl` shows "rejected" because the binary is unsigned, not because of quarantine.
* Removing `com.apple.quarantine` attributes doesn't help.
* `codesign --force -s -` (ad-hoc sign) fixes it.
* This happens for 3.13.2 on Sequoia;


**Possible fix**

* Strip `com.apple.quarantine` attributes and ad-hoc sign binaries at install time.
* Or document that users on macOS ≥ Sequoia must run `codesign -s -` on uv-installed interpreters.

**Additional note**
* I first observed this inside VS Code.
* It may be specific to interpreters launched from sandboxed GUI apps on macOS,  but I have not confirmed whether it also occurs when running the binary  directly from a terminal.

### Platform

macOS 15.5 (24F74)

### Version

uv 0.6.5

### Python version

python 3.13

---

_Label `bug` added by @ouatu-ro on 2025-09-23 15:23_

---

_Comment by @zanieb on 2025-09-23 16:03_

Thanks for the report.

cc @geofft 

---
