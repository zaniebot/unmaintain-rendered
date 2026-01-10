```yaml
number: 9294
title: Exclude failing sphinx file in ecosytem checks
type: pull_request
state: merged
author: zanieb
labels:
  - ci
assignees: []
merged: true
base: main
head: zb/ecosystem-sphinx
created_at: 2023-12-27T17:10:01Z
updated_at: 2023-12-28T16:42:08Z
url: https://github.com/astral-sh/ruff/pull/9294
synced_at: 2026-01-10T23:07:18Z
```

# Exclude failing sphinx file in ecosytem checks

---

_Pull request opened by @zanieb on 2023-12-27 17:10_

Failing due to

> error: Failed to read tests/roots/test-pycode/cp_1251_coded.py: stream did not contain valid UTF-8

Unclear to me if ignoring is the correct response.

---

_Label `ci` added by @zanieb on 2023-12-27 17:10_

---

_Comment by @github-actions[bot] on 2023-12-27 17:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: 'quote-style = preserve' is a preview only feature. Run with '--preview' to enable it.
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: 'quote-style = preserve' is a preview only feature. Run with '--preview' to enable it.
```

</p>
</details>

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser approved on 2023-12-28 10:18_

IMO: It's the right response to continue getting value from the ecosystem. 

Whether it's worth reconsidering our standpoint on supporting Python's encoding headers is a separate question (that, interestingly, hasn't come up as a user-reported issue).

Code wise: Neat how flexible the ecosystem checks are :)

---

_Merged by @zanieb on 2023-12-28 16:42_

---

_Closed by @zanieb on 2023-12-28 16:42_

---

_Branch deleted on 2023-12-28 16:42_

---
