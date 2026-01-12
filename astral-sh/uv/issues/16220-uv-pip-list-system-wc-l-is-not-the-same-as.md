```yaml
number: 16220
title: "`uv pip list --system | wc -l` is not the same as `python3 -m pip list | wc -l`, is this expected?"
type: issue
state: open
author: pplmx
labels:
  - question
assignees: []
created_at: 2025-10-10T07:35:09Z
updated_at: 2025-10-15T13:23:54Z
url: https://github.com/astral-sh/uv/issues/16220
synced_at: 2026-01-12T16:02:26Z
```

# `uv pip list --system | wc -l` is not the same as `python3 -m pip list | wc -l`, is this expected?

---

_@pplmx_

### Question

**Summary:**

`uv pip list --system | wc -l` reports significantly fewer packages than `python3 -m pip list | wc -l`, even though both point to the same Python installation.
Is this expected behavior?

---

### 🔍 Steps to Reproduce

```bash
root@userx:/var/tmp# uv self version
uv 0.9.1

root@userx:/var/tmp# uv python list --only-installed
cpython-3.10.18-linux-x86_64-gnu    /usr/local/bin/python3.10
cpython-3.10.18-linux-x86_64-gnu    /usr/local/bin/python3 -> /usr/local/bin/python3.10

root@userx:/var/tmp# command -v python3
/usr/local/bin/python3

root@userx:/var/tmp# uv pip list --system | wc -l
Using Python 3.10.18 environment at: /usr/local
45

root@userx:/var/tmp# python3 -m pip list | wc -l
202
```

---

### 💡 Expected Behavior

Both commands should list the same set of installed packages, since `uv pip list --system` claims to use the same system Python environment (`/usr/local`).


### Platform

Ubuntu 20.04.6 LTS

### Version

uv 0.9.1

---

_Label `question` added by @pplmx on 2025-10-10 07:35_

---

_Comment by @konstin on 2025-10-14 12:17_

Can you check which packages those are, and where they are installed?

---

_Comment by @pplmx on 2025-10-15 02:58_

I found something that might explain the issue:

```bash
# Compare outputs between pip and uv
pip show vllm

# ...
Location: /usr/local/custom/lib64/python3/dist-packages
# ...

uv pip show vllm
Using Python 3.10.18 environment at: /usr/local
warning: Package(s) not found for: vllm

# Check sys.path
python3 -c "import sys; from pprint import pprint; pprint(sys.path)"
['',
 '/usr/local/custom/lib64/python3/dist-packages',
 '/usr/local/lib/python310.zip',
 '/usr/local/lib/python3.10',
 '/usr/local/lib/python3.10/lib-dynload',
 '/usr/local/lib/python3.10/site-packages']
```

It seems that `uv pip list` doesn’t include packages located in `/usr/local/custom/lib64/python3/dist-packages`.

Could it be that this custom path isn’t being picked up in `sys.path` by `uv`?



---

_Comment by @konstin on 2025-10-15 11:47_

`uv pip list` currently sees only the main site-packages directory, not all for `sys.path`, this is a known limitation (for now).

---

_Comment by @konstin on 2025-10-15 13:00_

This is tracked in https://github.com/astral-sh/uv/issues/2500

---

_Comment by @pplmx on 2025-10-15 13:23_

> `uv pip list` currently sees only the main site-packages directory, not all for `sys.path`, this is a known limitation (for now).

Thanks for your explanation. :)

---
