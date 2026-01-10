```yaml
number: 9335
title: "Treat all `typing_extensions` members as typing aliases"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ext
created_at: 2023-12-31T16:47:09Z
updated_at: 2023-12-31T18:23:34Z
url: https://github.com/astral-sh/ruff/pull/9335
synced_at: 2026-01-10T23:07:18Z
```

# Treat all `typing_extensions` members as typing aliases

---

_Pull request opened by @charliermarsh on 2023-12-31 16:47_

## Summary

Historically, we encoded this list by extracting the `__all__`. I went to update it, but... is there really any value in it? Seems easier to just treat `typing_extensions` as an alias for `typing`.

Closes https://github.com/astral-sh/ruff/issues/9334.

---

_Label `bug` added by @charliermarsh on 2023-12-31 16:47_

---

_Review requested from @zanieb by @charliermarsh on 2023-12-31 16:47_

---

_Review requested from @konstin by @charliermarsh on 2023-12-31 16:47_

---

_Converted to draft by @charliermarsh on 2023-12-31 16:50_

---

_Comment by @charliermarsh on 2023-12-31 16:50_

Sorry, marking as draft, not done (but need to step out).

---

_Marked ready for review by @charliermarsh on 2023-12-31 16:51_

---

_Comment by @github-actions[bot] on 2023-12-31 17:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2023-12-31 17:10_

Since `typing_extensions==4.7.0`, [we've re-exported all objects from the `typing` module in `typing_extensions`](https://github.com/python/typing_extensions/blob/main/CHANGELOG.md#release-470rc1-june-21-2023) so that it can be used as a drop-in replacement for the `typing` module. So this makes sense to me!

---

_Comment by @charliermarsh on 2023-12-31 18:23_

Awesome, thank you! Gonna take that as an approval :)

---

_Merged by @charliermarsh on 2023-12-31 18:23_

---

_Closed by @charliermarsh on 2023-12-31 18:23_

---

_Branch deleted on 2023-12-31 18:23_

---
