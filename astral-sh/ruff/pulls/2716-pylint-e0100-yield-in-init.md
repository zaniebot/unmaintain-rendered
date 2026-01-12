```yaml
number: 2716
title: "pylint:  E0100 yield-in-init"
type: pull_request
state: merged
author: tomecki
labels:
  - rule
assignees: []
merged: true
base: main
head: main
created_at: 2023-02-10T09:15:33Z
updated_at: 2023-02-10T21:15:16Z
url: https://github.com/astral-sh/ruff/pull/2716
synced_at: 2026-01-12T15:55:10Z
```

# pylint:  E0100 yield-in-init

---

_@tomecki_

Rule ref:
https://pylint.pycqa.org/en/latest/user_guide/messages/error/init-is-generator.html

Pylint tracking issue: https://github.com/charliermarsh/ruff/issues/970

---

_Comment by @not-my-profile on 2023-02-10 09:57_

If we decide to established the naming convention suggested by #2712 the rule should rather be called `yield-in-init` since "allow init is generator" isn't proper English.

---

_Comment by @tomecki on 2023-02-10 14:22_

@not-my-profile / @charliermarsh renamed to `yield-in-init`

---

_Renamed from "pylint:  E0100 init-is-generator" to "pylint:  E0100 yield-in-init" by @tomecki on 2023-02-10 14:22_

---

_Comment by @charliermarsh on 2023-02-10 14:58_

Confusingly I see this:

![Screen Shot 2023-02-10 at 9 58 11 AM](https://user-images.githubusercontent.com/1309177/218123220-388c12bd-2f73-4c90-b005-ec0df179d0a3.png)

And yet I'm unable to push edits to the branch 🤔 


---

_Label `rule` added by @charliermarsh on 2023-02-10 21:15_

---

_Merged by @charliermarsh on 2023-02-10 21:15_

---

_Closed by @charliermarsh on 2023-02-10 21:15_

---
