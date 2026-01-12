```yaml
number: 1005
title: Implement PLE0206
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: PLE0206
created_at: 2022-12-03T04:49:16Z
updated_at: 2022-12-03T05:14:35Z
url: https://github.com/astral-sh/ruff/pull/1005
synced_at: 2026-01-12T15:55:05Z
```

# Implement PLE0206

---

_@harupy_

#970 

---

_@charliermarsh reviewed on 2022-12-03 04:55_

---

_Review comment by @charliermarsh on `src/pylint/plugins.rs`:18 on 2022-12-03 04:55_

Maybe also check `checker.is_builtin(property)`?

---

_Comment by @charliermarsh on 2022-12-03 05:04_

Rock! Nice work.

---

_Merged by @charliermarsh on 2022-12-03 05:04_

---

_Closed by @charliermarsh on 2022-12-03 05:04_

---

_Comment by @charliermarsh on 2022-12-03 05:05_

Oh wait, I think this should be `PLR0206`? Since it's a "refactor" rule?

---

_Comment by @harupy on 2022-12-03 05:07_

@charliermarsh I misunderstood the naming rule. Yes, it should be `PLR0206`.

---

_Comment by @charliermarsh on 2022-12-03 05:08_

@harupy - Sorry, I wasn't super clear. What do you think of that convention? I wanted to preserve the `R0206` part to match Pylint.

---

_Comment by @harupy on 2022-12-03 05:12_

That convention sounds good to me :) If we use `PLE`, `W1301` and `E1301` would conflict.

---

_Comment by @charliermarsh on 2022-12-03 05:13_

Oh hah, true!

---

_Comment by @charliermarsh on 2022-12-03 05:14_

I'll rename this check and push it straight to `main`.

---

_Comment by @charliermarsh on 2022-12-03 05:14_

Oh nm! Thank you!

---
