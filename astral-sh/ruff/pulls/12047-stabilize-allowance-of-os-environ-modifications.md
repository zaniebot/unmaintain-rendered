```yaml
number: 12047
title: Stabilize allowance of os.environ modifications between imports
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: ruff-0.5
head: charlie/e402
created_at: 2024-06-26T15:13:30Z
updated_at: 2024-06-26T15:28:30Z
url: https://github.com/astral-sh/ruff/pull/12047
synced_at: 2026-01-10T21:56:00Z
```

# Stabilize allowance of os.environ modifications between imports

---

_Pull request opened by @charliermarsh on 2024-06-26 15:13_

## Summary

See: https://github.com/astral-sh/ruff/pull/10066.


---

_Label `rule` added by @charliermarsh on 2024-06-26 15:13_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-06-26 15:13_

---

_@AlexWaygood approved on 2024-06-26 15:16_

---

_Merged by @charliermarsh on 2024-06-26 15:24_

---

_Closed by @charliermarsh on 2024-06-26 15:24_

---

_Branch deleted on 2024-06-26 15:24_

---

_Comment by @github-actions[bot] on 2024-06-26 15:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zproject/test_settings.py#L21'>zproject/test_settings.py:21:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zproject/test_settings.py#L22'>zproject/test_settings.py:22:1:</a> E402 Module level import not at top of file
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

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
