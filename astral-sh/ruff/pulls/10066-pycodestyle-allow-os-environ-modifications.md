```yaml
number: 10066
title: "[`pycodestyle`] Allow `os.environ` modifications between imports (`E402`)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: charlie/sys
created_at: 2024-02-20T18:02:41Z
updated_at: 2024-02-20T18:24:28Z
url: https://github.com/astral-sh/ruff/pull/10066
synced_at: 2026-01-12T15:55:31Z
```

# [`pycodestyle`] Allow `os.environ` modifications between imports (`E402`)

---

_@charliermarsh_

## Summary

Allows, e.g.:

```python
import os

os.environ["WORLD_SIZE"] = "1"
os.putenv("CUDA_VISIBLE_DEVICES", "4")

import torch
```

For now, this is only allowed in preview.

Closes https://github.com/astral-sh/ruff/issues/10059


---

_Label `rule` added by @charliermarsh on 2024-02-20 18:02_

---

_Label `preview` added by @charliermarsh on 2024-02-20 18:02_

---

_Comment by @github-actions[bot] on 2024-02-20 18:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/0c6dfd1e5e86af66eb001e54b7810257c55cd07f/zproject/test_settings.py#L21'>zproject/test_settings.py:21:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/zulip/zulip/blob/0c6dfd1e5e86af66eb001e54b7810257c55cd07f/zproject/test_settings.py#L22'>zproject/test_settings.py:22:1:</a> E402 Module level import not at top of file
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E402 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-02-20 18:24_

---

_Closed by @charliermarsh on 2024-02-20 18:24_

---

_Branch deleted on 2024-02-20 18:24_

---
