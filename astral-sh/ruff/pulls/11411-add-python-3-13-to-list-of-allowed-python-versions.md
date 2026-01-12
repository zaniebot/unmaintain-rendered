```yaml
number: 11411
title: Add Python 3.13 to list of allowed Python versions
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/py313
created_at: 2024-05-13T16:15:09Z
updated_at: 2024-05-13T17:09:26Z
url: https://github.com/astral-sh/ruff/pull/11411
synced_at: 2026-01-12T15:55:38Z
```

# Add Python 3.13 to list of allowed Python versions

---

_@charliermarsh_

## Summary

I believe we're already "Python 3.13-ready"? The main Ruff-impacting change I see in https://docs.python.org/3.13/whatsnew/3.13.html is [PEP 696](https://peps.python.org/pep-0696/) which Jelle added in https://github.com/astral-sh/ruff/pull/11120.


---

_Review requested from @MichaReiser by @charliermarsh on 2024-05-13 16:15_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-05-13 16:15_

---

_Review requested from @dhruvmanila by @charliermarsh on 2024-05-13 16:15_

---

_@zanieb approved on 2024-05-13 16:17_

---

_@dhruvmanila approved on 2024-05-13 16:28_

---

_Merged by @charliermarsh on 2024-05-13 16:35_

---

_Closed by @charliermarsh on 2024-05-13 16:35_

---

_Branch deleted on 2024-05-13 16:35_

---

_Comment by @github-actions[bot] on 2024-05-13 16:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @AlexWaygood on 2024-05-13 17:07_

I guess one other thing we could do is update UP035 to emit a diagnostic if `--target-version=py313` has been selected and a user imports `typing.TypeIs`, `warnings.deprecated`, `typing.ReadOnly`, `typing.NoDefault` `typing.get_protocol_members` or `typing.is_protocol` from `typing_extensions` rather than `typing`

---

_Comment by @charliermarsh on 2024-05-13 17:08_

Makes sense, mind filing an issue (or PRing it)? :)

---

_Comment by @AlexWaygood on 2024-05-13 17:09_

> Makes sense, mind filing an issue (or PRing it)? :)

#11413

---
