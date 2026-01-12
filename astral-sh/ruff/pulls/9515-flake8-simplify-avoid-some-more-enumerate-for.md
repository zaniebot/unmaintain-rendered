```yaml
number: 9515
title: "[`flake8-simplify`] Avoid some more `enumerate-for-loop` false positives (`SIM113`)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/local
created_at: 2024-01-14T17:21:24Z
updated_at: 2024-01-14T18:02:14Z
url: https://github.com/astral-sh/ruff/pull/9515
synced_at: 2026-01-12T15:55:29Z
```

# [`flake8-simplify`] Avoid some more `enumerate-for-loop` false positives (`SIM113`)

---

_@charliermarsh_

Avoids, e.g., [this false positive](https://github.com/zulip/zulip/blob/a4fad5dda116048e22c3d559d370367e4e1c9b9a/zerver/data_import/slack.py#L634) from the ecosystem check.

---

_Label `bug` added by @charliermarsh on 2024-01-14 17:21_

---

_Comment by @github-actions[bot] on 2024-01-14 17:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -3 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/a4fad5dda116048e22c3d559d370367e4e1c9b9a/zerver/data_import/slack.py#L634'>zerver/data_import/slack.py:634:9:</a> SIM113 Use `enumerate()` for index variable `recipient_id_count` in `for` loop
- <a href='https://github.com/zulip/zulip/blob/a4fad5dda116048e22c3d559d370367e4e1c9b9a/zerver/data_import/slack.py#L635'>zerver/data_import/slack.py:635:9:</a> SIM113 Use `enumerate()` for index variable `subscription_id_count` in `for` loop
- <a href='https://github.com/zulip/zulip/blob/a4fad5dda116048e22c3d559d370367e4e1c9b9a/zilencer/management/commands/populate_db.py#L705'>zilencer/management/commands/populate_db.py:705:17:</a> SIM113 Use `enumerate()` for index variable `i` in `for` loop
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM113 | 3 | 0 | 3 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-01-14 18:02_

---

_Closed by @charliermarsh on 2024-01-14 18:02_

---

_Branch deleted on 2024-01-14 18:02_

---
