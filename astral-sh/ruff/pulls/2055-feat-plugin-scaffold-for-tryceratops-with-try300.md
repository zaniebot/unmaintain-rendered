```yaml
number: 2055
title: "feat: plugin scaffold for tryceratops with TRY300"
type: pull_request
state: merged
author: sbrugman
labels: []
assignees: []
merged: true
base: main
head: feat-tryceratops
created_at: 2023-01-21T11:49:54Z
updated_at: 2023-01-21T16:25:10Z
url: https://github.com/astral-sh/ruff/pull/2055
synced_at: 2026-01-12T15:55:07Z
```

# feat: plugin scaffold for tryceratops with TRY300

---

_@sbrugman_

Renamed to TRY to avoid conflicts, as proposed in https://github.com/guilatrova/tryceratops/pull/55

https://github.com/guilatrova/tryceratops/blob/main/docs/violations/TC300.md

See: #2056

---

_Converted to draft by @sbrugman on 2023-01-21 12:07_

---

_Marked ready for review by @sbrugman on 2023-01-21 13:01_

---

_Comment by @charliermarsh on 2023-01-21 14:55_

Cool, this looks good. Just for planning and coordination, do you plan on implementing more of the rules from this plugin?

---

_Comment by @sbrugman on 2023-01-21 15:06_

Yes, I'm working on [TC004](https://github.com/guilatrova/tryceratops/blob/main/docs/violations/TC004.md): Prefer TypeError exception for invalid type - can do in separate PR so that others can see/pick others if they like
(This is a mistake developers can easily make from my experience)


---

_Merged by @charliermarsh on 2023-01-21 16:25_

---

_Closed by @charliermarsh on 2023-01-21 16:25_

---
