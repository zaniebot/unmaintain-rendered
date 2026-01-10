```yaml
number: 10964
title: "[`flake8-bandit`] Allow `urllib.request.urlopen` calls with static `Request` argument"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/s
created_at: 2024-04-16T02:06:13Z
updated_at: 2024-04-16T02:35:37Z
url: https://github.com/astral-sh/ruff/pull/10964
synced_at: 2026-01-10T22:37:01Z
```

# [`flake8-bandit`] Allow `urllib.request.urlopen` calls with static `Request` argument

---

_Pull request opened by @charliermarsh on 2024-04-16 02:06_

## Summary

Allows, e.g.:

```python
import urllib

urllib.request.urlopen(urllib.request.Request("https://example.com/"))
```

...in [`suspicious-url-open-usage`](https://docs.astral.sh/ruff/rules/suspicious-url-open-usage/).

See: https://github.com/astral-sh/ruff/issues/7918#issuecomment-2057661054



---

_Label `rule` added by @charliermarsh on 2024-04-16 02:06_

---

_Comment by @github-actions[bot] on 2024-04-16 02:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@zanieb approved on 2024-04-16 02:26_

---

_Merged by @charliermarsh on 2024-04-16 02:30_

---

_Closed by @charliermarsh on 2024-04-16 02:30_

---

_Branch deleted on 2024-04-16 02:30_

---
