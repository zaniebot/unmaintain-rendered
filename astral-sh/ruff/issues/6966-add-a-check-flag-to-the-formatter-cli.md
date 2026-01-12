```yaml
number: 6966
title: "Add a `--check` flag to the formatter CLI"
type: issue
state: closed
author: charliermarsh
labels:
  - formatter
assignees: []
created_at: 2023-08-29T03:44:31Z
updated_at: 2023-09-02T08:23:10Z
url: https://github.com/astral-sh/ruff/issues/6966
synced_at: 2026-01-12T15:54:46Z
```

# Add a `--check` flag to the formatter CLI

---

_@charliermarsh_

Avoids writing the files to disk, and exits with a non-zero status code if any files would be changed.

---

_Label `formatter` added by @charliermarsh on 2023-08-29 03:44_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-29 03:44_

---

_Comment by @MichaReiser on 2023-08-29 05:37_

We may need to wait with this until we decided on the formatter cli integration 

---

_Label `needs-decision` added by @MichaReiser on 2023-08-29 05:37_

---

_Comment by @charliermarsh on 2023-08-29 12:46_

I mostly have it done - any objection to moving it forward?

---

_Comment by @MichaReiser on 2023-08-29 12:48_

No, go for it, I mainly wanted to avoid unnecessary work. 

---

_Label `needs-decision` removed by @dhruvmanila on 2023-08-29 13:25_

---

_Comment by @zanieb on 2023-08-29 13:49_

I propose this flag anyway :)

---

_Closed by @charliermarsh on 2023-08-29 16:40_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-09-02 08:23_

---
