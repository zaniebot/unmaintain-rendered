---
number: 12752
title: uv pip compile requires pyproject.toml name exactly - why?
type: issue
state: closed
author: ihelmer07
labels:
  - question
assignees: []
created_at: 2025-04-08T16:57:52Z
updated_at: 2025-04-08T18:10:51Z
url: https://github.com/astral-sh/uv/issues/12752
synced_at: 2026-01-10T01:25:24Z
---

# uv pip compile requires pyproject.toml name exactly - why?

---

_Issue opened by @ihelmer07 on 2025-04-08 16:57_

### Summary

## Background
`uv pip compile` requires a file named `pyproject.toml` to be in current directory (or others like requirements.ini)
if no `pyproject.toml` is found uv provides the following error:
```
~/temp/uv2$ uv pip compile -o app.lock 
error: the following required arguments were not provided:
  <SRC_FILE|--group <GROUP>>
```
this would lead one to believe they can provide any SRC_FILE and have it work, which is not the case.

## Problem
If one wants to have a `pyproject.toml` that isn't named `pyproject.toml` (e.g. multiple `pyproject.toml` in the same folder (e.g. `app-pyproject.toml` vs `base-pyproject.toml`)) *that is properly formatted pyproject.toml* `uv pip compile` will not parse correctly.

```
~/temp/uv2$ uv pip compile -o app.lock app-pyproject.toml
error: Unexpected '[', expected '-c', '-e', '-r' or the start of a requirement at pyprojecst.toml:2:1
```
Renaming `app-pyproject.toml` to `pyproject.toml` fixes it.

**why is the manual renaming of files needed?**

## Steps to recreate

1. Create standard pyproject.toml named `app-pyproject.toml`
```
# app-pyproject.toml
[project]
name="app"
requires-python = ">=3.11"
version="0.1.0"
dependencies = [
  "numpy>=2.2.4",
  "polars==1.26",
]

```
2. Run `uv pip compile -o app.lock app-pyproject.toml`
3. See error
4. Rename `app-pyproject.toml` to `pyproject.toml`
5. run `uv pip compile -o app.lock pyproject.toml`
6. success

### Platform

any

### Version

0.6.13

### Python version

3.11

---

_Label `bug` added by @ihelmer07 on 2025-04-08 16:57_

---

_Comment by @zanieb on 2025-04-08 17:04_

`pyproject.toml` is the official standard name for the file format. We don't assume all toml files are `pyproject.toml` formatted. I think we're pretty unlikely to support alternative names without a strong justification.

---

_Label `bug` removed by @zanieb on 2025-04-08 17:04_

---

_Label `question` added by @zanieb on 2025-04-08 17:04_

---

_Comment by @zanieb on 2025-04-08 17:04_

> If one wants to have a pyproject.toml that isn't named pyproject.toml (e.g. multiple pyproject.toml in the same folder (e.g. app-pyproject.toml vs base-pyproject.toml)) that is properly formatted pyproject.toml uv pip compile will not parse correctly.

This will break lots of tooling. I'd recommend not doing that if you can avoid it.

---

_Comment by @ihelmer07 on 2025-04-08 17:46_

@zanieb - I agree that it will break a lot of tooling, I just don't understand why if the user explicitly states the `.toml` file it won't parse correctly? From my understanding only `requirements.ini`, `requirements.txt` and `pyproject.toml` are valid for `uv pip compile`. why is uv forcing the filename?

---

_Comment by @zanieb on 2025-04-08 18:04_

There are other `.toml` files, e.g., the new lockfile format from PEP 741 is `pylock.toml` and will be supported here. The name is used to declare the schema of the file.

---

_Comment by @ihelmer07 on 2025-04-08 18:10_

Got it - thanks very much!

---

_Closed by @ihelmer07 on 2025-04-08 18:10_

---
