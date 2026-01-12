```yaml
number: 1489
title: "Add a \"fix message\" to every autofix-able check"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/fix-message
created_at: 2022-12-30T22:50:19Z
updated_at: 2022-12-31T04:16:05Z
url: https://github.com/astral-sh/ruff/pull/1489
synced_at: 2026-01-12T15:55:06Z
```

# Add a "fix message" to every autofix-able check

---

_@charliermarsh_

This PR adds meaningful messages to the `Fix` API.

Prior to this change, the "Quick Fix" actions in VS Code and elsewhere used opaque titles like "Fix F401". Now, they give the user some indication of the _content_ of the fix.

Before:

![Screen Shot 2022-12-30 at 10 58 34 PM](https://user-images.githubusercontent.com/1309177/210124176-d55fdc72-a98d-4870-bb78-1c8a16811cb3.png)
![Screen Shot 2022-12-30 at 10 58 54 PM](https://user-images.githubusercontent.com/1309177/210124177-95df5bce-81c5-47d8-b140-de4b4948d87f.png)

After:

![Screen Shot 2022-12-30 at 11 04 32 PM](https://user-images.githubusercontent.com/1309177/210124324-92a77c3b-af00-41b4-9929-12b433e79c33.png)
![Screen Shot 2022-12-30 at 10 56 34 PM](https://user-images.githubusercontent.com/1309177/210124131-eed145f5-7555-43a6-bd0b-802577f3b7f3.png)

Resolves #1454.

---

_@charliermarsh reviewed on 2022-12-30 23:51_

---

_Review comment by @charliermarsh on `src/checks.rs`:3349 on 2022-12-30 23:51_

Need to finish these, adjust the playground to respect them, then merge.

---

_Renamed from "Add message to fix API" to "Add a "fix message" to every autofix-able check" by @charliermarsh on 2022-12-31 03:55_

---

_Merged by @charliermarsh on 2022-12-31 04:16_

---

_Closed by @charliermarsh on 2022-12-31 04:16_

---

_Branch deleted on 2022-12-31 04:16_

---
