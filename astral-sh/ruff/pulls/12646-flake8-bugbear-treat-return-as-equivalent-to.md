```yaml
number: 12646
title: "[`flake8-bugbear`] Treat return as equivalent to break (`B909`)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/return
created_at: 2024-08-02T21:43:25Z
updated_at: 2024-08-02T22:14:19Z
url: https://github.com/astral-sh/ruff/pull/12646
synced_at: 2026-01-12T15:55:41Z
```

# [`flake8-bugbear`] Treat return as equivalent to break (`B909`)

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/12640.

---

_Label `bug` added by @charliermarsh on 2024-08-02 21:43_

---

_Comment by @github-actions[bot] on 2024-08-02 21:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -1 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/a8bdd0db91d4f3b6060127dc7a0adba383e4e999/rotkehlchen/api/rest.py#L526'>rotkehlchen/api/rest.py:526:130:</a> RUF100 [*] Unused `noqa` directive (unused: `B909`)
+ <a href='https://github.com/rotki/rotki/blob/a8bdd0db91d4f3b6060127dc7a0adba383e4e999/rotkehlchen/chain/evm/decoding/decoder.py#L926'>rotkehlchen/chain/evm/decoding/decoder.py:926:44:</a> RUF100 [*] Unused `noqa` directive (unused: `B909`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pytest-dev/pytest/blob/c79f815070e31600732e5e17bad1dcfc57703426/src/_pytest/pytester.py#L310'>src/_pytest/pytester.py:310:17:</a> B909 Mutation to loop iterable `self.calls` during iteration
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 2 | 2 | 0 | 0 | 0 |
| B909 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-08-02 22:14_

---

_Closed by @charliermarsh on 2024-08-02 22:14_

---

_Branch deleted on 2024-08-02 22:14_

---
