```yaml
number: 9778
title: "Feature request: add `DEPRECATED` selector"
type: issue
state: closed
author: bersbersbers
labels:
  - configuration
assignees: []
created_at: 2024-02-02T08:27:31Z
updated_at: 2024-03-11T16:47:33Z
url: https://github.com/astral-sh/ruff/issues/9778
synced_at: 2026-01-10T11:09:51Z
```

# Feature request: add `DEPRECATED` selector

---

_Issue opened by @bersbersbers on 2024-02-02 08:27_

`ruff check --isolated --select=ALL` selects `ANN101` which is deprecated. I would love to be able to do `ruff check --isolated --select=ALL --ignore=DEPRECATED`. (I know I could do `ruff check --isolated --select=ALL --preview`, but I don't want to add all preview rules.)

---

_Label `configuration` added by @AlexWaygood on 2024-02-02 08:51_

---

_Comment by @bersbersbers on 2024-02-29 17:49_

Another great selector would be `INCOMPATIBLE_WITH_FORMATTER`, including (for example) `COM812`.

---

_Renamed from "Feature request: add `DEPRECATED` selector" to "Feature request: add `DEPRECATED` and `INCOMPATIBLE_WITH_FORMATTER` selectors" by @bersbersbers on 2024-02-29 17:49_

---

_Comment by @MichaReiser on 2024-03-04 11:43_

Sounds reasonable to me. I rename this back to `DEPRECATED` and track the formatter incompatible rules in https://github.com/astral-sh/ruff/issues/10225

---

_Renamed from "Feature request: add `DEPRECATED` and `INCOMPATIBLE_WITH_FORMATTER` selectors" to "Feature request: add `DEPRECATED` selector" by @MichaReiser on 2024-03-04 11:43_

---

_Comment by @zanieb on 2024-03-11 16:46_

I think we should do https://github.com/astral-sh/ruff/issues/10342 instead

---

_Closed by @zanieb on 2024-03-11 16:46_

---

_Comment by @zanieb on 2024-03-11 16:47_

By the way, if you don't want to enable all preview rules, you can use [`explicit-preview-rules`](https://docs.astral.sh/ruff/settings/#lint_explicit-preview-rules).

---
