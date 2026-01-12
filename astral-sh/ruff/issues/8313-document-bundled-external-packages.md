```yaml
number: 8313
title: Document bundled external packages
type: issue
state: open
author: stefan6419846
labels:
  - release
  - needs-decision
assignees: []
created_at: 2023-10-28T20:32:59Z
updated_at: 2023-10-30T23:30:12Z
url: https://github.com/astral-sh/ruff/issues/8313
synced_at: 2026-01-12T15:54:48Z
```

# Document bundled external packages

---

_@stefan6419846_

*ruff* seems to bundle quite a lot of external dependencies/packages in its binary distributions (looking at https://github.com/astral-sh/ruff/network/dependencies and the `Cargo.lock` entries, over 200?) There is no clear overview of all corresponding inclusions and their licenses.

While linting tools like *ruff* tend to usually not be shipped with any closed source product, they might require license compliance scanning as well in some situations.

It would be great if *ruff* would provide this information inside its wheel files as well (in a similar manner to directly included code).

---

_Label `release` added by @charliermarsh on 2023-10-30 23:30_

---

_Label `needs-decision` added by @charliermarsh on 2023-10-30 23:30_

---
