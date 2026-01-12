```yaml
number: 2657
title: "feat(isort): Implement known-local-folder"
type: pull_request
state: merged
author: spaceone
labels:
  - isort
  - configuration
assignees: []
merged: true
base: main
head: isort-known-local-folder
created_at: 2023-02-08T13:31:34Z
updated_at: 2023-02-10T18:15:34Z
url: https://github.com/astral-sh/ruff/pull/2657
synced_at: 2026-01-12T15:55:09Z
```

# feat(isort): Implement known-local-folder

---

_@spaceone_

sub item of #2419 

to make #2419 really happen some more refactoring has to be done by outsourcing the sections into mapping.

---

_@spaceone reviewed on 2023-02-08 13:38_

---

_Review comment by @spaceone on `crates/ruff/src/rules/isort/categorize.rs`:54 on 2023-02-08 13:38_

I didn't understand this one - a new name is necessary. Suggestions?

---

_@charliermarsh reviewed on 2023-02-10 02:34_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/isort/categorize.rs`:59 on 2023-02-10 02:34_

Doesn't this cause us to render the `.` imports and the "known local folder" imports as separate sections? Does that match isort's behavior? If so I find it a little surprising.

---

_@charliermarsh reviewed on 2023-02-10 03:33_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/isort/categorize.rs`:59 on 2023-02-10 03:33_

Like, I would've expected that we remove `ImportType::Relative`, and just use `ImportType::LocalFolder` in both places.

---

_Label `isort` added by @charliermarsh on 2023-02-10 04:09_

---

_Label `configuration` added by @charliermarsh on 2023-02-10 04:09_

---

_Review comment by @spaceone on `crates/ruff/src/rules/isort/categorize.rs`:59 on 2023-02-10 17:08_

yes, done.

---

_@spaceone reviewed on 2023-02-10 17:08_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/isort/categorize.rs`:59 on 2023-02-10 17:08_

Thx

---

_@charliermarsh reviewed on 2023-02-10 17:08_

---

_Merged by @charliermarsh on 2023-02-10 18:15_

---

_Closed by @charliermarsh on 2023-02-10 18:15_

---
