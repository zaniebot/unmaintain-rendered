```yaml
number: 12499
title: "Initial `uv add --script` produces invalid result with existing metadata section"
type: issue
state: closed
author: merlinz01
labels:
  - bug
assignees: []
created_at: 2025-03-26T23:05:51Z
updated_at: 2025-03-27T21:39:30Z
url: https://github.com/astral-sh/uv/issues/12499
synced_at: 2026-01-10T03:41:47Z
```

# Initial `uv add --script` produces invalid result with existing metadata section

---

_Issue opened by @merlinz01 on 2025-03-26 23:05_

### Summary

When running `uv add --script <script> <package>` on a script which has an existing non-`script` metadata section and doesn't have the `script` section, uv fails to put a blank line between the sections, resulting in an unintuitive error when running the script or adding more dependencies.

MRE:
```bash
$ cat myscript.py
# /// nonstandardsection
# Hello,
# World!
# ///

print('Hello, world')

$ uv run --script myscript.py 
Hello, world
$ uv add --script myscript.py requests
Updated `myscript.py`
$ cat myscript.py 
# /// script
# requires-python = ">=3.13"
# dependencies = [
#     "requests",
# ]
# ///
# /// nonstandardsection
# Hello,
# World!
# ///

print('Hello, world')

$ uv run --script myscript.py 
error: TOML parse error at line 5, column 1
  |
5 | ///
  | ^
invalid key
$ uv add --script myscript.py fastapi
error: TOML parse error at line 5, column 1
  |
5 | ///
  | ^
invalid key
$ nano myscript.py # add a blank line between the sections
$ cat myscript.py 
# /// script
# requires-python = ">=3.13"
# dependencies = [
#     "requests",
# ]
# ///

# /// nonstandardsection
# Hello,
# World!
# ///

print('Hello, world')

$ uv run --script myscript.py 
Installed 5 packages in 6ms
Hello, world
```

Possibly related: #6366

### Platform

Linux Mint 22.1

### Version

uv 0.6.9

### Python version

Python 3.13.0

---

_Label `bug` added by @merlinz01 on 2025-03-26 23:05_

---

_Comment by @merlinz01 on 2025-03-26 23:35_

Looking at the source code, maybe add another newline here https://github.com/j178/uv/blob/e0f81f0d4a904d8c743e776d9f8c9ef5b96f769c/crates/uv-scripts/src/lib.rs#L533-L553
and some logic here https://github.com/j178/uv/blob/e0f81f0d4a904d8c743e776d9f8c9ef5b96f769c/crates/uv-scripts/src/lib.rs#L430

---

_Comment by @merlinz01 on 2025-03-26 23:36_

It only does this when the nonstandard section is the first line in the file.

---

_Comment by @merlinz01 on 2025-03-26 23:40_

More accurately, it does this when the first non-shebang line in the file is a valid metadata section line.

---

_Closed by @charliermarsh on 2025-03-27 21:39_

---

_Closed by @charliermarsh on 2025-03-27 21:39_

---
