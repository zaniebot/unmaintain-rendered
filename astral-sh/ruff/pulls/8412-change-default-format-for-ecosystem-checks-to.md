```yaml
number: 8412
title: "Change default format for ecosystem checks to `markdown`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zanie/default-output-eco
created_at: 2023-11-01T17:14:35Z
updated_at: 2023-11-01T21:51:35Z
url: https://github.com/astral-sh/ruff/pull/8412
synced_at: 2026-01-10T23:40:55Z
```

# Change default format for ecosystem checks to `markdown`

---

_Pull request opened by @zanieb on 2023-11-01 17:14_

To facilitate easier local runs


---

_Label `internal` added by @zanieb on 2023-11-01 21:00_

---

_@charliermarsh reviewed on 2023-11-01 21:28_

---

_Review comment by @charliermarsh on `python/ruff-ecosystem/README.md`:34 on 2023-11-01 21:28_

Which one is particularly useful when developing? Markdown or JSON?

---

_@charliermarsh approved on 2023-11-01 21:28_

---

_Review comment by @charliermarsh on `python/ruff-ecosystem/ruff_ecosystem/cli.py`:124 on 2023-11-01 21:28_

Do we need to pass this in somewhere now?

---

_@charliermarsh reviewed on 2023-11-01 21:28_

---

_@zanieb reviewed on 2023-11-01 21:29_

---

_Review comment by @zanieb on `python/ruff-ecosystem/ruff_ecosystem/cli.py`:124 on 2023-11-01 21:29_

No in CI we pass `--output-format markdown` already 

---

_@zanieb reviewed on 2023-11-01 21:30_

---

_Review comment by @zanieb on `python/ruff-ecosystem/README.md`:34 on 2023-11-01 21:30_

```suggestion
The default output format is markdown, which includes nice summaries of the changes. You can use `--output-format json` to display the raw data — this is
```

---

_@zanieb reviewed on 2023-11-01 21:31_

---

_Review comment by @zanieb on `python/ruff-ecosystem/README.md`:35 on 2023-11-01 21:31_

```suggestion
particularly useful when making changes to the ecosystem checks. 
```

---

_Merged by @zanieb on 2023-11-01 21:51_

---

_Closed by @zanieb on 2023-11-01 21:51_

---

_Branch deleted on 2023-11-01 21:51_

---
