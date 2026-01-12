```yaml
number: 7508
title: "Display alternative Python implementations in `uv python list`"
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zb/pypy-fix
created_at: 2024-09-18T16:38:07Z
updated_at: 2024-09-23T22:37:17Z
url: https://github.com/astral-sh/uv/pull/7508
synced_at: 2026-01-12T16:07:52Z
```

# Display alternative Python implementations in `uv python list`

---

_@zanieb_

Closes #7286

We now show externally-managed alternative Python implementations

```
❯ ./target/debug/uv python list --only-installed
cpython-3.13.0rc2-macos-aarch64-none    /Users/zb/.local/share/uv/python/cpython-3.13.0rc2-macos-aarch64-none/bin/python3 -> python3.13
cpython-3.12.6-macos-aarch64-none       /opt/homebrew/opt/python@3.12/bin/python3.12 -> ../Frameworks/Python.framework/Versions/3.12/bin/python3.12
cpython-3.12.6-macos-aarch64-none       /Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3 -> python3.12
cpython-3.11.10-macos-aarch64-none      /opt/homebrew/opt/python@3.11/bin/python3.11 -> ../Frameworks/Python.framework/Versions/3.11/bin/python3.11
cpython-3.11.10-macos-aarch64-none      /Users/zb/.local/share/uv/python/cpython-3.11.10-macos-aarch64-none/bin/python3 -> python3.11
cpython-3.11.5-macos-aarch64-none       /Users/zb/.local/share/uv/python/cpython-3.11.5-macos-aarch64-none/bin/python3 -> python3.11
cpython-3.9.6-macos-aarch64-none        /Library/Developer/CommandLineTools/usr/bin/python3 -> ../../Library/Frameworks/Python3.framework/Versions/3.9/bin/python3
pypy-3.10.14-macos-aarch64-none         /opt/homebrew/bin/pypy3 -> ../Cellar/pypy3.10/7.3.17/bin/pypy3
pypy-3.10.14-macos-aarch64-none         /Users/zb/.local/share/uv/python/pypy-3.10.14-macos-aarch64-none/bin/python3 -> pypy3.10
```

Previously, this required the implementation to use the `python` executable name. The problem here is that we weren't looking at these executables at all unless the alternative implementation was requested.

---

_@zanieb reviewed on 2024-09-18 16:44_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:920 on 2024-09-18 16:44_

This is a bit annoying, but necessary to prevent a regression when you have an alternative implementation on your PATH as `python`. We might want to track executable names in `PythonSource::SearchPath` directly; then we couple implement this in `PythonSource::allows_alternative_implementations`

---

_@zanieb reviewed on 2024-09-18 16:45_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1579 on 2024-09-18 16:45_

cc @BurntSushi

This is the result of our failure to use an iterator here.

---

_@zanieb reviewed on 2024-09-23 20:21_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1579 on 2024-09-23 20:21_

Improving this in #7649 

---

_Comment by @zanieb on 2024-09-23 21:10_

Moving some of this into #7649 

---

_Comment by @zanieb on 2024-09-23 22:37_

I think this was fully superseded by #7517 and #7649 

---

_Closed by @zanieb on 2024-09-23 22:37_

---
