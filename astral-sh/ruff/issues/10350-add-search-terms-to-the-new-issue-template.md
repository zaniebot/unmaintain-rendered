```yaml
number: 10350
title: "(🎁) Add \"Search terms\" to the new issue template"
type: issue
state: closed
author: KotlinIsland
labels:
  - documentation
assignees: []
created_at: 2024-03-12T01:01:22Z
updated_at: 2024-03-13T03:32:45Z
url: https://github.com/astral-sh/ruff/issues/10350
synced_at: 2026-01-10T11:09:52Z
```

# (🎁) Add "Search terms" to the new issue template

---

_Issue opened by @KotlinIsland on 2024-03-12 01:01_

typescript has this super useful section in the issue template where you list searchable terms:

```yaml
- type: textarea
    id: search_terms
    attributes:
      label: '🔎 Search Terms'
      description: |
        What search terms did you use when trying to find an existing bug report?

        List them here so people in the future can find this one more easily.
      placeholder: |
        List of keywords you searched for before creating this issue. Write them down here so that others can find this bug more easily and help provide feedback.

        e.g. "function inference any", "jsx attribute spread", "move to file duplicate imports", "discriminated union inference", "ts2822"
    validations:
      required: true
```

---

_Comment by @charliermarsh on 2024-03-12 01:18_

Oh, that's a nice idea.

---

_Label `documentation` added by @charliermarsh on 2024-03-12 01:18_

---

_Closed by @zanieb on 2024-03-13 03:32_

---
