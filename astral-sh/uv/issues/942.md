```yaml
number: 942
title: "Add `--offline` flag to disallow network requests"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - wish
assignees: []
created_at: 2024-01-16T19:15:22Z
updated_at: 2025-03-17T07:49:11Z
url: https://github.com/astral-sh/uv/issues/942
synced_at: 2026-01-10T03:50:29Z
```

# Add `--offline` flag to disallow network requests

---

_Issue opened by @charliermarsh on 2024-01-16 19:15_

This is like `--no-index`, but it should disallow _all_ network requests (including for distributions and `--find-links`).

---

_Label `enhancement` added by @charliermarsh on 2024-01-16 19:15_

---

_Label `wish` added by @charliermarsh on 2024-01-16 19:15_

---

_Comment by @konstin on 2024-01-18 12:26_

What are the use cases for `--no-index` once we have an offline flag, are people vendoring all their deps into their repository?

---

_Comment by @charliermarsh on 2024-01-18 13:44_

The only thing I can think of is: you’re using find links and don’t want to use PyPI.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-06 00:19_

---

_Closed by @charliermarsh on 2024-02-13 03:35_

---

_Comment by @andrei-korshikov on 2025-03-17 07:49_

**`--offline` is not offline for "git+https" sources.**

I'm on uv 0.6.6 (c1a0bb85e 2025-03-12). Some of my dependencies are `git+https`, e.g. I have

```python
# /// script
# dependencies = [
#   "pycurl @ git+https://github.com/pycurl/pycurl.git",
#   ]
# ///
```

On every run of `uv run --offline my_script.py` I see `Resolving dependencies...` message and get these messages on `stderr` (and see corresponding traffic in Wireshark):

```
Updating https://github.com/pycurl/pycurl.git (HEAD)
Updated https://github.com/pycurl/pycurl.git (d92395d9fe1e18365c4e4812f4351bd6985872e1)
```

If this behaviour is intended, I think it should be documented. Documentation for `--offline` reads: "Disable network access. When disabled, uv will only use locally cached data and locally available files.", and as we see for "git+https" it is definitely not so.

---

Off-topic: and huge thank you for `uv`!:)

---
