```yaml
number: 2087
title: "feat: implement TRY200"
type: pull_request
state: merged
author: karpa4o4
labels: []
assignees: []
merged: true
base: main
head: feat/tryceratops-TRY200
created_at: 2023-01-22T14:42:55Z
updated_at: 2023-01-23T19:12:46Z
url: https://github.com/astral-sh/ruff/pull/2087
synced_at: 2026-01-12T15:55:07Z
```

# feat: implement TRY200

---

_@karpa4o4_

#2056

---

_Review comment by @charliermarsh on `src/rules/tryceratops/rules/reraise_no_cause.rs`:54 on 2023-01-22 17:59_

I think we need to recurse here to handle cases like:

```py
def bad():
    try:
        process()
    except MyException:
        if True:
            raise MainFunctionFailed()
```

This came up in another PR -- here's my explanation: https://github.com/charliermarsh/ruff/pull/2073#discussion_r1083345058

---

_@charliermarsh reviewed on 2023-01-22 17:59_

---

_@charliermarsh reviewed on 2023-01-22 17:59_

---

_Review comment by @charliermarsh on `src/rules/tryceratops/rules/reraise_no_cause.rs`:54 on 2023-01-22 17:59_

(Otherwise, looks good!)

---

_@charliermarsh requested changes on 2023-01-22 18:00_

Thanks! Think we just need to expand the logic to recurse over the statements.

---

_Comment by @charliermarsh on 2023-01-23 19:09_

Looks good, I'm gonna rebase then merge.

---

_Merged by @charliermarsh on 2023-01-23 19:12_

---

_Closed by @charliermarsh on 2023-01-23 19:12_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-01-23 19:12_

---
