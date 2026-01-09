---
number: 12273
title: Cache mismatch when installing a package for differents Python versions
type: issue
state: closed
author: framillien
labels:
  - question
assignees: []
created_at: 2025-03-18T10:50:14Z
updated_at: 2025-03-19T11:06:31Z
url: https://github.com/astral-sh/uv/issues/12273
synced_at: 2026-01-07T13:12:18-06:00
---

# Cache mismatch when installing a package for differents Python versions

---

_Issue opened by @framillien on 2025-03-18 10:50_

### Summary

When a Python wheel is bound to specific Python versions, the installed version may not match the current Python version.

Package [multiprocess](https://pypi.org/project/multiprocess/#files) is a wheel built for many Python versions:
- multiprocess-0.70.17-py310-none-any.whl
- multiprocess-0.70.17-py311-none-any.whl
- ...

When we install `multiprocess` in a Python 3.10 venv, and then in a Python 3.11 or 3.12 venv, `uv` reuse the cache entry from Python 3.10 despite all information in logs are pointing to the expected package. Going from a higher version to a lower version work as expected (3.12 then 3.10). `pip` install the expected version.

Know workarounds are:
- Add `--no-cache` on command line
- Clean the cache after each installation `uv cache clean multiprocess`

How to reproduce (tested on Linux RHEL8 and Ubuntu22):

```
% uv cache clean multiprocess
% uv venv --python 3.10 .venv3-10
% uv pip install --python .venv3-10/bin/python3 multiprocess
% uv venv --python 3.11 .venv3-11
% uv pip install --python .venv3-11/bin/python3 multiprocess
% cat .venv3-11/lib/python3.11/site-packages/multiprocess-0.70.17.dist-info/WHEEL | grep Tag
Tag: py310-none-any

==> KO, must be py311 (same error if we activate the venv)

% uv cache clean multiprocess
% uv venv --python 3.11 .venv3-11
% uv pip install --python .venv3-11/bin/python3 multiprocess
% cat .venv3-11/lib/python3.11/site-packages/multiprocess-0.70.17.dist-info/WHEEL | grep Tag
Tag: py311-none-any

==> OK
```

Verbose log: [uv-multiprocess-mismatch.txt](https://github.com/user-attachments/files/19316799/uv-multiprocess-mismatch.txt)

### Platform

RHEL 8, Ubuntu 22

### Version

uv 0.6.7

### Python version

3.10, 3.11, 3.12

---

_Label `bug` added by @framillien on 2025-03-18 10:50_

---

_Comment by @charliermarsh on 2025-03-18 13:41_

I think this might be correct, though. `multiprocess-0.70.17-py310-none-any.whl` means: "Any Python implementation with version 3.10 or later." That tag does not exclude Python 3.11.

Notice that pip will happily install it:

```
❯ pip --version
pip 22.3.1 from /usr/local/lib/python3.11/site-packages/pip (python 3.11)

❯ pip install https://files.pythonhosted.org/packages/e7/a9/39cf856d03690af6fd570cf40331f1f79acdbb3132a9c35d2c5002f7f30b/multiprocess-0.70.17-py310-none-any.whl
Collecting multiprocess==0.70.17
  Downloading multiprocess-0.70.17-py310-none-any.whl (134 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 134.8/134.8 kB 3.0 MB/s eta 0:00:00
...

❯ pip install https://files.pythonhosted.org/packages/c8/b7/2e9a4fcd871b81e1f2a812cd5c6fb52ad1e8da7bf0d7646c55eaae220484/multiprocess-0.70.17-py313-none-any.whl
ERROR: multiprocess-0.70.17-py313-none-any.whl is not a supported wheel on this platform
```

---

_Label `bug` removed by @charliermarsh on 2025-03-18 13:41_

---

_Label `question` added by @charliermarsh on 2025-03-18 13:41_

---

_Comment by @framillien on 2025-03-18 18:51_

Hi Charlie, and thanks for the quick answer. Agree that we can force PIP do install a specific version, but:
- Does this behaviour be consistent in `uv` with or without cache ?
- If `uv` resolve a specific version from indexes, does this version be installed regardless of the cache state ?
- Does this behaviour be consistent with `pip` way to install packages.

In the log linked in the description, we see that `uv` resolve the expected version, but the cache install another one:
```
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a4/69/d3f343a61a2f86ef10ed7865a26beda7c71554136ce187b0384b1c2c9ca3/multiprocess-0.70.17-py312-none-any.whl.metadata
```

I think that the `bug` label is relevant for above reasons.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-18 20:56_

---

_Referenced in [astral-sh/uv#12301](../../astral-sh/uv/pulls/12301.md) on 2025-03-18 22:16_

---

_Comment by @charliermarsh on 2025-03-18 22:16_

It seems reasonable to change this: https://github.com/astral-sh/uv/pull/12301

---

_Closed by @charliermarsh on 2025-03-19 01:41_

---

_Closed by @charliermarsh on 2025-03-19 01:41_

---

_Comment by @framillien on 2025-03-19 11:06_

Thank you so much for this quick change. I confirm this is resolved. 👍 

---
