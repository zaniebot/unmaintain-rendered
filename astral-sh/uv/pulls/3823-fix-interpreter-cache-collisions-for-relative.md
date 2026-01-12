```yaml
number: 3823
title: Fix interpreter cache collisions for relative virtualenv paths
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-relative-venv
created_at: 2024-05-24T14:41:09Z
updated_at: 2024-05-27T08:53:48Z
url: https://github.com/astral-sh/uv/pull/3823
synced_at: 2026-01-12T16:05:52Z
```

# Fix interpreter cache collisions for relative virtualenv paths

---

_@zanieb_

Closes #3784

The cache did not use an absolute path. I'm not sure this is actually a new bug, as this code wasn't touched in #3266 but perhaps there was a slight difference in the paths we were passing around. Note, just canonicalizing the path as soon as we see it doesn't work because then we jump out of the virtual environmnent into the system interpreter.

## Test plan

```
❯ uv venv
Using Python 3.12.3 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
❯ uv pip install anyio
Resolved 3 packages in 81ms
Installed 3 packages in 4ms
 + anyio==4.3.0
 + idna==3.7
 + sniffio==1.3.1
❯ mkdir uv-issue-3784 && cd uv-issue-3784
❯ uv venv
Using Python 3.12.3 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

❯ gcm
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
❯ cargo run -q -- pip list -v -p .venv
DEBUG Checking for Python interpreter in directory `.venv`
TRACE Cached interpreter info for Python 3.12.3, skipping probing: .venv/bin/python3
DEBUG Using Python 3.12.3 environment at .venv/bin/python3
Package Version
------- -------
anyio   4.3.0
idna    3.7
sniffio 1.3.1
❯ cd uv-issue-3784
❯ cargo run -q -- pip list -v -p .venv
DEBUG Checking for Python interpreter in directory `.venv`
TRACE Cached interpreter info for Python 3.12.3, skipping probing: .venv/bin/python3
DEBUG Using Python 3.12.3 environment at /Users/zb/workspace/uv/.venv/bin/python3
Package Version
------- -------
anyio   4.3.0
idna    3.7
sniffio 1.3.1

❯ cd ..
❯ gco zb/fix-relative-venv
Switched to branch 'zb/fix-relative-venv'
❯ cargo run -q -- pip list -v -p .venv
DEBUG Checking for Python interpreter in directory `.venv`
TRACE Cached interpreter info for Python 3.12.3, skipping probing: .venv/bin/python3
DEBUG Using Python 3.12.3 environment at .venv/bin/python3
Package Version
------- -------
anyio   4.3.0
idna    3.7
sniffio 1.3.1
❯ cd uv-issue-3784
❯ cargo run -q -- pip list -v -p .venv
DEBUG Checking for Python interpreter in directory `.venv`
TRACE Cached interpreter info for Python 3.12.3, skipping probing: .venv/bin/python3
DEBUG Using Python 3.12.3 environment at .venv/bin/python3
```

---

_Label `bug` added by @zanieb on 2024-05-24 14:41_

---

_@konstin approved on 2024-05-24 16:17_

---

_Merged by @zanieb on 2024-05-24 16:18_

---

_Closed by @zanieb on 2024-05-24 16:18_

---

_Branch deleted on 2024-05-24 16:18_

---

_Comment by @ydshieh on 2024-05-27 08:53_

Hi

Should we still apply the suggestion mentioned

https://github.com/astral-sh/uv/issues/3784#issuecomment-2129554631

We still get the same issue with 0.2.4

---
