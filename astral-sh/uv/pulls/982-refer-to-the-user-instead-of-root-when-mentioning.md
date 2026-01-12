```yaml
number: 982
title: "Refer to the user instead of \"root\" when mentioning direct dependencies"
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/root-require
created_at: 2024-01-19T00:02:18Z
updated_at: 2024-01-19T17:17:43Z
url: https://github.com/astral-sh/uv/pull/982
synced_at: 2026-01-12T16:04:20Z
```

# Refer to the user instead of "root" when mentioning direct dependencies

---

_@zanieb_

Closes https://github.com/astral-sh/puffin/issues/857


---

_Label `error messages` added by @zanieb on 2024-01-19 00:25_

---

_@charliermarsh reviewed on 2024-01-19 03:43_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_install_scenarios.rs`:2370 on 2024-01-19 03:43_

(Nicer as: "because you require b and a", or "because you require both b and a")

---

_@charliermarsh reviewed on 2024-01-19 03:44_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_compile.rs`:693 on 2024-01-19 03:44_

Unrelated: missing a comma before the `we` here (but it's present in the line before).

---

_@charliermarsh approved on 2024-01-19 03:45_

I'm mixed on "you" (and second-person in general here) but don't know that I have better suggestions (it's better than "root").

---

_Comment by @zanieb on 2024-01-19 04:55_

> I'm mixed on "you" (and second-person in general here) but don't know that I have better suggestions (it's better than "root").

`pip` uses "the user" which I find too impersonal. I generally try to avoid "you", but it seems like the best option here. Alternatively we could do "black was requested" or "black is a direct requirement" instead of "you require black" but I think those have their own problems.

---

_Merged by @zanieb on 2024-01-19 17:17_

---

_Closed by @zanieb on 2024-01-19 17:17_

---

_Branch deleted on 2024-01-19 17:17_

---
