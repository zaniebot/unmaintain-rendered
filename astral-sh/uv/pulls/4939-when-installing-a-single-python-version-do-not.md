```yaml
number: 4939
title: When installing a single Python version, do not display number installed
type: pull_request
state: closed
author: zanieb
labels:
  - cli
  - preview
assignees: []
base: main
head: zb/install-message
created_at: 2024-07-09T19:39:33Z
updated_at: 2024-07-18T19:10:36Z
url: https://github.com/astral-sh/uv/pull/4939
synced_at: 2026-01-10T13:42:52Z
```

# When installing a single Python version, do not display number installed

---

_Pull request opened by @zanieb on 2024-07-09 19:39_

e.g.,

```
Searching for Python versions matching: any Python
Installed Python 3.12.3 to: /Users/zb/Library/Application Support/uv/python/cpython-3.12.3-macos-aarch64-none
Installed Python in 1.80s
```

instead of `Installed 1 version in 1.80s`

---

_Label `cli` added by @zanieb on 2024-07-09 19:39_

---

_Label `preview` added by @zanieb on 2024-07-09 19:44_

---

_Comment by @T-256 on 2024-07-09 20:00_

```diff
 Searching for Python versions matching: any Python
-Installed Python 3.12.3 to: /Users/zb/Library/Application Support/uv/python/cpython-3.12.3-macos-aarch64-none
-Installed Python in 1.80s
+Installed `cpython-3.12.3-macos-aarch64-none` in 1.80s
```
Getting install location is available by `uv python dir`, so I think it's better to prevent leak username for privacy 🤷‍♂️ 

---

_Comment by @zanieb on 2024-07-09 20:06_

Could be nice to use the key there. Hm.

Idk about the install paths, we might need to consider that more holistically as part of #4896 as we do this all over the place right now.

---

_Comment by @T-256 on 2024-07-09 20:58_

```
Installed 1 python version in 25ms
 + cpython-3.12.3-macos-aarch64-none
```
Also, I think it's worth to match it to output design of `uv pip install` which is more stabled.

---

_Review requested from @charliermarsh by @charliermarsh on 2024-07-18 18:52_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-18 18:53_

---

_Comment by @charliermarsh on 2024-07-18 19:09_

Will take over.

---

_Closed by @zanieb on 2024-07-18 19:10_

---
