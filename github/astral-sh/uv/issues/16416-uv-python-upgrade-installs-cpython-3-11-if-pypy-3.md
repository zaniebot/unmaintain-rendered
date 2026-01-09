---
number: 16416
title: "`uv python upgrade` installs cpython-3.11 if pypy-3.11 exists"
type: issue
state: closed
author: injust
labels:
  - bug
assignees: []
created_at: 2025-10-23T08:14:04Z
updated_at: 2025-10-23T15:48:35Z
url: https://github.com/astral-sh/uv/issues/16416
synced_at: 2026-01-07T13:12:19-06:00
---

# `uv python upgrade` installs cpython-3.11 if pypy-3.11 exists

---

_Issue opened by @injust on 2025-10-23 08:14_

### Summary

```
$ uv python list
cpython-3.15.0a1-macos-x86_64-none                 <download available>
cpython-3.15.0a1+freethreaded-macos-x86_64-none    <download available>
cpython-3.14.0-macos-x86_64-none                   /usr/local/bin/python3.14 -> ../Cellar/python@3.14/3.14.0/bin/python3.14
cpython-3.14.0-macos-x86_64-none                   /usr/local/bin/python3 -> ../Cellar/python@3.14/3.14.0/bin/python3
cpython-3.14.0-macos-x86_64-none                   .local/bin/python3.14 -> .local/share/uv/python/cpython-3.14.0-macos-x86_64-none/bin/python3.14
cpython-3.14.0-macos-x86_64-none                   .local/bin/python3 -> .local/share/uv/python/cpython-3.14.0-macos-x86_64-none/bin/python3.14
cpython-3.14.0-macos-x86_64-none                   .local/bin/python -> .local/share/uv/python/cpython-3.14.0-macos-x86_64-none/bin/python3.14
cpython-3.14.0-macos-x86_64-none                   .local/share/uv/python/cpython-3.14.0-macos-x86_64-none/bin/python3.14
cpython-3.14.0+freethreaded-macos-x86_64-none      <download available>
cpython-3.13.9-macos-x86_64-none                   <download available>
cpython-3.13.9+freethreaded-macos-x86_64-none      <download available>
cpython-3.12.12-macos-x86_64-none                  <download available>
cpython-3.11.14-macos-x86_64-none                  <download available>
cpython-3.10.19-macos-x86_64-none                  <download available>
cpython-3.9.24-macos-x86_64-none                   <download available>
cpython-3.9.6-macos-x86_64-none                    /usr/bin/python3
cpython-3.8.20-macos-x86_64-none                   <download available>
pypy-3.11.13-macos-x86_64-none                     .local/bin/python3.11 -> .local/share/uv/python/pypy-3.11.13-macos-x86_64-none/bin/pypy3.11
pypy-3.11.13-macos-x86_64-none                     .local/bin/pypy3 -> .local/share/uv/python/pypy-3.11.13-macos-x86_64-none/bin/pypy3.11
pypy-3.11.13-macos-x86_64-none                     .local/bin/pypy -> .local/share/uv/python/pypy-3.11.13-macos-x86_64-none/bin/pypy3.11
pypy-3.11.13-macos-x86_64-none                     .local/share/uv/python/pypy-3.11.13-macos-x86_64-none/bin/pypy3.11
pypy-3.10.16-macos-x86_64-none                     <download available>
pypy-3.9.19-macos-x86_64-none                      <download available>
pypy-3.8.16-macos-x86_64-none                      <download available>
graalpy-3.12.0-macos-x86_64-none                   <download available>
graalpy-3.11.0-macos-x86_64-none                   <download available>
graalpy-3.10.0-macos-x86_64-none                   <download available>
graalpy-3.8.5-macos-x86_64-none                    <download available>

$ uv python upgrade
warning: `uv python upgrade` is experimental and may change without warning. Pass `--preview-features python-upgrade` to disable this warning
Installed Python 3.11.14 in 2.98s
 + cpython-3.11.14-macos-x86_64-none
```

### Platform

macOS 15 x86_64

### Version

uv 0.9.5 (Homebrew 2025-10-21)

### Python version

_No response_

---

_Label `bug` added by @injust on 2025-10-23 08:14_

---

_Comment by @zanieb on 2025-10-23 13:08_

Thanks for the report!

---

_Comment by @zanieb on 2025-10-23 13:09_

A quick diagnosis... it looks like https://github.com/astral-sh/uv/blob/51e8da2d1c7a15cd30901de565fba2c19413f6da/crates/uv/src/commands/python/install.rs#L215 doesn't construct a request including the implementation, which is problematic.

---

_Referenced in [astral-sh/uv#16420](../../astral-sh/uv/pulls/16420.md) on 2025-10-23 14:57_

---

_Closed by @zanieb on 2025-10-23 15:48_

---
