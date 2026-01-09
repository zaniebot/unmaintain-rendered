---
number: 8165
title: "Ignore `S324` if an insecure hash function is called with `kwargs`"
type: issue
state: closed
author: harupy
labels:
  - rule
assignees: []
created_at: 2023-10-24T13:26:28Z
updated_at: 2023-10-24T23:56:24Z
url: https://github.com/astral-sh/ruff/issues/8165
synced_at: 2026-01-07T13:12:15-06:00
---

# Ignore `S324` if an insecure hash function is called with `kwargs`

---

_Issue opened by @harupy on 2023-10-24 13:26_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

### Example code

```python
import hashlib
import sys

HASHLIB_KWARGS = (
    {"usedforsecurity": False} if sys.version_info >= (3, 9) else {}
)
hashlib.md5(**HASHLIB_KWARGS)
# ^^^^^^^^^
# S324
```

Can we ignore `S324` in this case?

---

_Comment by @harupy on 2023-10-24 13:26_

Related to https://github.com/mlflow/mlflow/issues/10106

---

_Comment by @zanieb on 2023-10-24 16:36_

Seems reasonable to me 👍 

---

_Label `rule` added by @zanieb on 2023-10-24 16:36_

---

_Comment by @Avasam on 2023-10-24 21:25_

This should definitely be well documented though. It wasn't immediately obvious to me why using kwargs makes this secure:

> Changed in version 3.9: All hashlib constructors take a keyword-only argument usedforsecurity with default value True. A false value allows the use of insecure and blocked hashing algorithms in restricted environments. False indicates that the hashing algorithm is not used in a security context, e.g. as a non-cryptographic one-way compression function.

Meaning if your dict doesn't include `usedforsecurity` then it stays default `True` and will be prevented in certain contexts. `usedforsecurity=False` assumes that you explicitely "approved" the use of insecure hash. **Unless the dict comes from user/external input**.

> Changed in version 3.9

Also meaning it should still raise for Python < 3.9

---

_Comment by @charliermarsh on 2023-10-24 21:26_

I'm mixed on this, I'd only want to allow it if we can statically verify that the `**kwargs` resolve in this way, so as to avoid introducing false negatives for security-related warnings.

---

_Comment by @harupy on 2023-10-24 23:56_

Thanks for the comments! I agree. `hashlib.md5(**kwargs)` doesn't necessarily mean kwargs has `usedforsecurity=False`. Closing.

---

_Closed by @harupy on 2023-10-24 23:56_

---
