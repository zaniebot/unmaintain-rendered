```yaml
number: 16170
title: "`~/.local/bin/python3.10` points to `pypy3.10`"
type: issue
state: open
author: balthild
labels:
  - bug
assignees: []
created_at: 2025-10-08T01:32:34Z
updated_at: 2025-10-08T04:12:02Z
url: https://github.com/astral-sh/uv/issues/16170
synced_at: 2026-01-12T16:02:25Z
```

# `~/.local/bin/python3.10` points to `pypy3.10`

---

_@balthild_

### Summary

When pypy is installed, the symlink of same python minor version in `~/local/bin` is overridden.

```
❯ uv python list
cpython-3.14.0rc2-macos-aarch64-none                 <download available>
cpython-3.14.0rc2+freethreaded-macos-aarch64-none    <download available>
cpython-3.13.7-macos-aarch64-none                    <download available>
cpython-3.13.7+freethreaded-macos-aarch64-none       <download available>
cpython-3.12.11-macos-aarch64-none                   <download available>
cpython-3.12.8-macos-aarch64-none                    /Users/balthild/.local/share/uv/python/cpython-3.12.8-macos-aarch64-none/bin/python3.12
cpython-3.11.13-macos-aarch64-none                   <download available>
cpython-3.10.18-macos-aarch64-none                   <download available>
cpython-3.10.12-macos-aarch64-none                   /Users/balthild/.local/share/uv/python/cpython-3.10.12-macos-aarch64-none/bin/python3.10
cpython-3.9.23-macos-aarch64-none                    <download available>
cpython-3.9.6-macos-aarch64-none                     /usr/bin/python3
cpython-3.8.20-macos-aarch64-none                    <download available>
pypy-3.11.13-macos-aarch64-none                      <download available>
pypy-3.10.16-macos-aarch64-none                      /Users/balthild/.local/bin/python3.10 -> /Users/balthild/.local/share/uv/python/pypy-3.10.16-macos-aarch64-none/bin/pypy3.10
pypy-3.10.16-macos-aarch64-none                      /Users/balthild/.local/share/uv/python/pypy-3.10.16-macos-aarch64-none/bin/pypy3.10
pypy-3.9.19-macos-aarch64-none                       <download available>
pypy-3.8.16-macos-aarch64-none                       <download available>
graalpy-3.11.0-macos-aarch64-none                    <download available>
graalpy-3.10.0-macos-aarch64-none                    <download available>
graalpy-3.8.5-macos-aarch64-none                     <download available>
```

### Platform

macOS 15.6 arm64

### Version

uv 0.8.14

### Python version

Python 3.12.8

---

_Label `bug` added by @balthild on 2025-10-08 01:32_

---

_Comment by @zanieb on 2025-10-08 04:12_

See #14201

---
