```yaml
number: 19260
title: Rename the msrv job to indicate it tests
type: pull_request
state: closed
author: zanieb
labels:
  - ci
assignees: []
base: main
head: zb/msrv
created_at: 2025-07-10T13:07:09Z
updated_at: 2025-07-10T13:57:39Z
url: https://github.com/astral-sh/ruff/pull/19260
synced_at: 2026-01-12T15:56:35Z
```

# Rename the msrv job to indicate it tests

---

_@zanieb_

Otherwise, this is pretty confusing on failure as I am expecting a compile error

---

_Label `ci` added by @zanieb on 2025-07-10 13:07_

---

_Comment by @github-actions[bot] on 2025-07-10 13:18_

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

_Comment by @zanieb on 2025-07-10 13:23_

We'd need to disable the msrv required check for a day or so then roll it over to this name

---

_@MichaReiser approved on 2025-07-10 13:36_

 I'm not sure if it even needs to run the tests. All that's really required is that it builds the tests to ensure the tests and source both compile. Either way. Renaming seems fine.

---

_Comment by @zanieb on 2025-07-10 13:40_

I don't mind changing it to just build the tests, if that's the intent. 

---

_Closed by @zanieb on 2025-07-10 13:57_

---
