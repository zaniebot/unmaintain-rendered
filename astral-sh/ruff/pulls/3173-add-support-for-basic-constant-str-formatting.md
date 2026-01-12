```yaml
number: 3173
title: "Add support for basic `Constant::Str` formatting"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/constants
created_at: 2023-02-23T16:08:25Z
updated_at: 2023-02-23T16:23:12Z
url: https://github.com/astral-sh/ruff/pull/3173
synced_at: 2026-01-12T04:39:44Z
```

# Add support for basic `Constant::Str` formatting

---

_Pull request opened by @charliermarsh on 2023-02-23 16:08_

This PR enables us to apply the proper quotation marks, including support for escapes. There are some significant TODOs, especially around implicit concatenations like:

```py
(
  "abc"
  "def"
)
```

Which are represented as a single AST node, which requires us to tokenize _within_ the formatter to identify all the individual string parts.


---

_Label `autoformatter` added by @charliermarsh on 2023-02-23 16:08_

---

_Merged by @charliermarsh on 2023-02-23 16:23_

---

_Closed by @charliermarsh on 2023-02-23 16:23_

---

_Branch deleted on 2023-02-23 16:23_

---
