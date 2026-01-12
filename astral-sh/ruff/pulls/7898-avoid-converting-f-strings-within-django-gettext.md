```yaml
number: 7898
title: "Avoid converting f-strings within Django `gettext` calls"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/f-string-django
created_at: 2023-10-10T16:21:51Z
updated_at: 2023-10-10T16:37:38Z
url: https://github.com/astral-sh/ruff/pull/7898
synced_at: 2026-01-12T15:55:25Z
```

# Avoid converting f-strings within Django `gettext` calls

---

_@charliermarsh_

## Summary

Django's `gettext` doesn't support f-strings, so we should avoid translating `.format` calls in those cases.

Closes https://github.com/astral-sh/ruff/issues/7891.


---

_Comment by @charliermarsh on 2023-10-10 16:26_

Some more resources:

- https://savannah.gnu.org/bugs/?61596
- https://groups.google.com/g/django-developers/c/XNhpOHam0zE (apparently the issue, more precisely, is that Python doesn't let you replace the string prior to the f-string evaluation, or something)

---

_Merged by @charliermarsh on 2023-10-10 16:31_

---

_Closed by @charliermarsh on 2023-10-10 16:31_

---

_Branch deleted on 2023-10-10 16:31_

---

_Comment by @github-actions[bot] on 2023-10-10 16:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
