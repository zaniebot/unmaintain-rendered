```yaml
number: 2227
title: Add RUF999 adding noqa comments
type: issue
state: closed
author: spaceone
labels: []
assignees: []
created_at: 2023-01-26T21:32:57Z
updated_at: 2023-01-26T21:36:46Z
url: https://github.com/astral-sh/ruff/issues/2227
synced_at: 2026-01-10T11:09:45Z
```

# Add RUF999 adding noqa comments

---

_Issue opened by @spaceone on 2023-01-26 21:32_

A new code e.g. `RUF999` in combination with `--select` would be helpful which just adds `# noqa: XXX`  statements to findings of the selected codes.
Of course it should be excluded from `ALL`.

---

_Comment by @charliermarsh on 2023-01-26 21:35_

Have you tried `ruff --add-noqa --select ...`?

---

_Comment by @charliermarsh on 2023-01-26 21:36_

(It may not do exactly what you want, but curious if it works for this.)

---

_Comment by @spaceone on 2023-01-26 21:36_

oh crazy! thanks!

---

_Closed by @spaceone on 2023-01-26 21:36_

---
