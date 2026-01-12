```yaml
number: 10728
title: "Respect `Q00*` ignores in `flake8-quotes` rules"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/q
created_at: 2024-04-02T03:01:02Z
updated_at: 2024-04-02T03:44:17Z
url: https://github.com/astral-sh/ruff/pull/10728
synced_at: 2026-01-12T15:55:33Z
```

# Respect `Q00*` ignores in `flake8-quotes` rules

---

_@charliermarsh_

## Summary

We lost the per-rule ignores when these were migrated to the AST, so if _any_ `Q` rule is enabled, they're now all enabled.

Closes https://github.com/astral-sh/ruff/issues/10724.

## Test Plan

Ran:

```shell
ruff check . --isolated --select Q --ignore Q000
ruff check . --isolated --select Q --ignore Q001
ruff check . --isolated --select Q --ignore Q002
ruff check . --isolated --select Q --ignore Q000,Q001
ruff check . --isolated --select Q --ignore Q000,Q002
ruff check . --isolated --select Q --ignore Q001,Q002
```

...against:

```python
'''
bad docsting
'''
a = 'single'
b = '''
bad multi line
'''
```


---

_Label `bug` added by @charliermarsh on 2024-04-02 03:01_

---

_Merged by @charliermarsh on 2024-04-02 03:21_

---

_Closed by @charliermarsh on 2024-04-02 03:21_

---

_Branch deleted on 2024-04-02 03:21_

---

_Comment by @github-actions[bot] on 2024-04-02 03:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/indico/indico/blob/4d06f82cfe28f60b44136be90fff7c6f81bc593f/indico/modules/admin/notices.py#L40'>indico/modules/admin/notices.py:40:1:</a> UP042 Class NoticeSeverity inherits from both `str` and `enum.Enum`. Prefer `enum.StrEnum` instead.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP042 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/indico/indico/blob/4d06f82cfe28f60b44136be90fff7c6f81bc593f/indico/modules/admin/notices.py#L40'>indico/modules/admin/notices.py:40:1:</a> UP042 Class NoticeSeverity inherits from both `str` and `enum.Enum`. Prefer `enum.StrEnum` instead.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP042 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @dhruvmanila on 2024-04-02 03:44_

Thank you!

---
