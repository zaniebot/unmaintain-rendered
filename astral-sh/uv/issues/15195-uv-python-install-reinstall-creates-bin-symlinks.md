```yaml
number: 15195
title: uv python install --reinstall creates bin symlinks to 3.14.0a5 over betas or rc
type: issue
state: open
author: geofft
labels:
  - bug
assignees: []
created_at: 2025-08-09T19:34:43Z
updated_at: 2025-08-09T19:34:43Z
url: https://github.com/astral-sh/uv/issues/15195
synced_at: 2026-01-12T16:02:05Z
```

# uv python install --reinstall creates bin symlinks to 3.14.0a5 over betas or rc

---

_@geofft_

### Summary

```
$ ./uv python install --reinstall
Installed 19 versions in 1m 52s
 ~ cpython-3.8.20-linux-x86_64-gnu (python3.8)
 ~ cpython-3.9.12-linux-x86_64-gnu
 ~ cpython-3.9.20-linux-x86_64-gnu
 ~ cpython-3.9.21-linux-x86_64-gnu
 ~ cpython-3.9.22-linux-x86_64-gnu
 ~ cpython-3.9.23-linux-x86_64-gnu (python3.9)
 ~ cpython-3.10.16-linux-x86_64-gnu
 ~ cpython-3.10.18-linux-x86_64-gnu (python3.10)
 ~ cpython-3.11.11-linux-x86_64-gnu
 ~ cpython-3.11.13-linux-x86_64-gnu (python3.11)
 ~ cpython-3.12.9-linux-x86_64-gnu
 ~ cpython-3.12.10-linux-x86_64-gnu
 ~ cpython-3.12.11-linux-x86_64-gnu (python3.12)
 ~ cpython-3.13.0-linux-x86_64-gnu
 ~ cpython-3.13.5-linux-x86_64-gnu (python3.13)
 ~ cpython-3.14.0a5-linux-x86_64-gnu (python3.14)
 ~ cpython-3.14.0a6-linux-x86_64-gnu
 ~ cpython-3.14.0b2-linux-x86_64-gnu
 ~ cpython-3.14.0rc1-linux-x86_64-gnu
$ ls -l `which python3.14`
lrwxrwxrwx 1 ubuntu ubuntu 84 Aug  9 19:28 /home/ubuntu/.local/bin/python3.14 -> /home/ubuntu/.local/share/uv/python/cpython-3.14.0a5-linux-x86_64-gnu/bin/python3.14
$ ./uvx python@3.14 -c 'import sys, _tkinter; print(sys.version, _tkinter)'
3.14.0a5 (main, Mar 11 2025, 17:29:28) [Clang 20.1.0 ] <module '_tkinter' (built-in)>
```

Guessing we're missing an appropriate sort here? But it does seem to sort right for the prereleases.

cc @jtfmumm, @zanieb

### Platform

Ubuntu 24.04

### Version

the CI binary from #15179, which identifies as `uv 0.8.8 (bec354a06 2025-08-09)`

### Python version

the wrong one

---

_Label `bug` added by @geofft on 2025-08-09 19:34_

---
