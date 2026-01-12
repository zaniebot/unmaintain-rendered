```yaml
number: 2297
title: Added ability to select bytecode invalidation mode of generated .pyc
type: pull_request
state: merged
author: wimglenn
labels:
  - enhancement
assignees: []
merged: true
base: main
head: hash-based-invalidation
created_at: 2024-03-08T07:34:28Z
updated_at: 2024-03-08T17:28:12Z
url: https://github.com/astral-sh/uv/pull/2297
synced_at: 2026-01-12T16:04:58Z
```

# Added ability to select bytecode invalidation mode of generated .pyc

---

_@wimglenn_

Since Python 3.7, deterministic pycs are possible (see [PEP 552](https://peps.python.org/pep-0552/))
To select the bytecode invalidation mode explicitly by env var:

    PYC_INVALIDATION_MODE=UNCHECKED_HASH uv pip install --compile ...

Valid values are TIMESTAMP (default), CHECKED_HASH, and UNCHECKED_HASH.
The latter options are useful for reproducible builds.


---

_Comment by @konstin on 2024-03-08 08:42_

Thanks!

I added specific error handling and a test and fixed a bug the error handling surfaced.

```
$ PYC_INVALIDATION_MODE=bogus cargo run -q pip install --compile django
Resolved 3 packages in 4ms
Installed 3 packages in 45ms
error: Failed to bytecode compile /home/konsti/projects/uv/.venv/lib/python3.12/site-packages
  Caused by: Python process stderr:
Invalid value for PYC_INVALIDATION_MODE "bogus", valid are "TIMESTAMP", "CHECKED_HASH", "UNCHECKED_HASH":
  Caused by: Bytecode compilation failed, expected "/home/konsti/projects/uv/.venv/lib/python3.12/site-packages/asgiref/sync.py", received: ""
```

@charliermarsh and/or @zanieb: Do we have a good spot to document advanced non-cli configuration such as `PYC_INVALIDATION_MODE`?

---

_Label `enhancement` added by @konstin on 2024-03-08 08:43_

---

_@konstin approved on 2024-03-08 08:43_

---

_Comment by @konstin on 2024-03-08 11:20_

(Sorry for the CI failures, it's passing locally for me)

---

_Comment by @zanieb on 2024-03-08 15:33_

@konstin I'd just add a "Compilation of Python modules" section to the "Advanced Usage" part of the README and explain `--compile` and this environment variable.

---

_Comment by @konstin on 2024-03-08 15:58_

Made a separate issue for: https://github.com/astral-sh/uv/issues/2303

---

_Merged by @konstin on 2024-03-08 16:55_

---

_Closed by @konstin on 2024-03-08 16:55_

---

_Branch deleted on 2024-03-08 17:28_

---
