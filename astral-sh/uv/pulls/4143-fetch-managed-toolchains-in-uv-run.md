```yaml
number: 4143
title: "Fetch managed toolchains in `uv run`"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/toolchain-vi
created_at: 2024-06-07T20:06:01Z
updated_at: 2024-06-10T14:20:46Z
url: https://github.com/astral-sh/uv/pull/4143
synced_at: 2026-01-10T13:54:02Z
```

# Fetch managed toolchains in `uv run`

---

_Pull request opened by @zanieb on 2024-06-07 20:06_

e.g.
```
❯ cargo run -q -- run --python 3.8.18 --isolated --with anyio -v -- python3
warning: `uv run` is experimental and may change without warning.
DEBUG Syncing ephemeral environment.
DEBUG Searching for Python 3.8.18 in all sources
DEBUG Searching for managed toolchains at `/Users/zb/Library/Application Support/uv/toolchains`
DEBUG Found CPython 3.12.3 at `/opt/homebrew/bin/python3` (search path)
DEBUG Found CPython 3.9.6 at `/usr/bin/python3` (search path)
DEBUG Found CPython 3.12.3 at `/opt/homebrew/bin/python3` (search path)
DEBUG Requested Python not found, checking for available download...
DEBUG Using registry request timeout of 30s
INFO Fetching requested toolchain...
DEBUG Downloading https://github.com/indygreg/python-build-standalone/releases/download/20240224/cpython-3.8.18%2B20240224-aarch64-apple-darwin-pgo%2Blto-full.tar.zst to temporary location /Users/zb/Library/Application Support/uv/toolchains/.tmpnYw0tc
DEBUG Extracting cpython-3.8.18%2B20240224-aarch64-apple-darwin-pgo%2Blto-full.tar.zst
DEBUG Moving /Users/zb/Library/Application Support/uv/toolchains/.tmpnYw0tc/python to /Users/zb/Library/Application Support/uv/toolchains/cpython-3.8.18-macos-aarch64-none
INFO Ignoring empty directory
DEBUG At least one requirement is not satisfied: anyio
...
Installed 5 packages in 19ms
 + anyio==4.4.0
 + exceptiongroup==1.2.1
 + idna==3.7
 + sniffio==1.3.1
 + typing-extensions==4.12.2
DEBUG Running `python3`
Python 3.8.18 (default, Feb 25 2024, 04:25:37) 
[Clang 17.0.6 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

---

_Comment by @zanieb on 2024-06-07 20:06_

Requires `--with <blah>` right now because we don't check if the Python request is satisfied in the project environment? Will investigate that.

---

_Label `preview` added by @zanieb on 2024-06-07 20:06_

---

_Comment by @charliermarsh on 2024-06-07 20:11_

Are you looking for #3945?

---

_Comment by @zanieb on 2024-06-07 20:12_

@charliermarsh I bet I am! Thought I'd seen something about that :) Thanks!

---

_Marked ready for review by @zanieb on 2024-06-07 23:32_

---

_@charliermarsh approved on 2024-06-09 18:16_

---

_Merged by @zanieb on 2024-06-10 14:20_

---

_Closed by @zanieb on 2024-06-10 14:20_

---

_Branch deleted on 2024-06-10 14:20_

---
