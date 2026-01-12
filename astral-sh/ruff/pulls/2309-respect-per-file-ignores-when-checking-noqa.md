```yaml
number: 2309
title: Respect per-file-ignores when checking noqa
type: pull_request
state: merged
author: sciyoshi
labels: []
assignees: []
merged: true
base: main
head: per-file-ignore-noqa
created_at: 2023-01-28T19:08:11Z
updated_at: 2023-01-28T19:19:00Z
url: https://github.com/astral-sh/ruff/pull/2309
synced_at: 2026-01-12T15:55:07Z
```

# Respect per-file-ignores when checking noqa

---

_@sciyoshi_

`RUF100` does not take into account a rule ignored for a file via a `per-file-ignores` configuration. To see this, try the following pyproject.toml:

```toml
[tool.ruff.per-file-ignores]
"test.py" = ["F401"]
```

and this test.py file:

```python
import itertools  # noqa: F401
```

Running `ruff --extend-select RUF100 test.py`, we should expect to get this error:

```
test.py:1:19: RUF100 Unused `noqa` directive (unused: `F401`)
```

The issue is that the per-file-ignores diagnostics are filtered out after the noqa checks, rather than before.

---

_Comment by @charliermarsh on 2023-01-28 19:16_

Ah, good catch!

---

_Merged by @charliermarsh on 2023-01-28 19:16_

---

_Closed by @charliermarsh on 2023-01-28 19:16_

---

_Branch deleted on 2023-01-28 19:17_

---

_Comment by @sciyoshi on 2023-01-28 19:18_

Looking again, I think I may have put the filtering in the wrong spot - it should probably come after this block https://github.com/charliermarsh/ruff/pull/2309/files#diff-9f39c861d0ab3e1bd158b77ee65246fe5c438ab6f2ddd112684cc6cef6185f01L136-L150

---
