```yaml
number: 10884
title: Avoid TOCTOU errors in cache initialization
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/git
created_at: 2024-04-11T15:54:24Z
updated_at: 2024-04-11T16:09:09Z
url: https://github.com/astral-sh/ruff/pull/10884
synced_at: 2026-01-10T22:37:01Z
```

# Avoid TOCTOU errors in cache initialization

---

_Pull request opened by @charliermarsh on 2024-04-11 15:54_

## Summary

I believe this should close https://github.com/astral-sh/ruff/issues/10880? The `.gitignore` creation seems ok, since it truncates, but using `cachedir::is_tagged` followed by `cachedir::add_tag` is not safe, as `cachedir::add_tag` _fails_ if the file already exists.

This also matches the structure of the code in `uv`.

Closes https://github.com/astral-sh/ruff/issues/10880.


---

_Label `bug` added by @charliermarsh on 2024-04-11 15:54_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-04-11 15:54_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-04-11 15:55_

---

_@BurntSushi approved on 2024-04-11 16:05_

Makes sense to me!

---

_Comment by @github-actions[bot] on 2024-04-11 16:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/bbefde554d0b5bc5557ab30617b22f28f727f4c9/scripts/validate_unwanted_patterns.py#L428'>scripts/validate_unwanted_patterns.py:428:3:</a> E999 SyntaxError: unindent does not match any outer indentation level
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E999 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-04-11 16:09_

---

_Closed by @charliermarsh on 2024-04-11 16:09_

---

_Branch deleted on 2024-04-11 16:09_

---
