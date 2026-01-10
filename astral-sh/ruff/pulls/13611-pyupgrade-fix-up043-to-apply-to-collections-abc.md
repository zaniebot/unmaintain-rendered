```yaml
number: 13611
title: "[`pyupgrade`] Fix UP043 to apply to `collections.abc.Generator` and `collections.abc.AsyncGenerator`"
type: pull_request
state: merged
author: cake-monotone
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-up043-for-collections-abc
created_at: 2024-10-03T11:59:02Z
updated_at: 2024-10-03T14:51:03Z
url: https://github.com/astral-sh/ruff/pull/13611
synced_at: 2026-01-10T20:59:36Z
```

# [`pyupgrade`] Fix UP043 to apply to `collections.abc.Generator` and `collections.abc.AsyncGenerator`

---

_Pull request opened by @cake-monotone on 2024-10-03 11:59_

## Summary

fix #13602 

Currently, `UP043` only applies to typing.Generator, but it should also support collections.abc.Generator.

This update ensures `UP043` correctly handles both `collections.abc.Generator` and `collections.abc.AsyncGenerator`

### UP043
> `UP043`
> Python 3.13 introduced the ability for type parameters to specify default values. As such, the default type arguments for some types in the standard library (e.g., Generator, AsyncGenerator) are now optional.
> Omitting type parameters that match the default values can make the code more concise and easier to read.

```py
Generator[int, None, None] -> Generator[int]
```


---

_@charliermarsh approved on 2024-10-03 12:06_

Thanks, makes sense!

---

_Label `rule` added by @charliermarsh on 2024-10-03 12:06_

---

_Merged by @charliermarsh on 2024-10-03 12:06_

---

_Closed by @charliermarsh on 2024-10-03 12:06_

---

_Comment by @github-actions[bot] on 2024-10-03 12:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` removed by @charliermarsh on 2024-10-03 14:51_

---

_Label `bug` added by @charliermarsh on 2024-10-03 14:51_

---
