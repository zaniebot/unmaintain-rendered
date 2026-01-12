```yaml
number: 3160
title: "Don't touch tuple brackets after `in`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/tuple-brackets
created_at: 2023-02-23T03:07:58Z
updated_at: 2023-02-23T03:14:11Z
url: https://github.com/astral-sh/ruff/pull/3160
synced_at: 2026-01-12T04:39:44Z
```

# Don't touch tuple brackets after `in`

---

_Pull request opened by @charliermarsh on 2023-02-23 03:07_

_No description provided._

---

_Renamed from "Don't touch tuple brackets after in" to "Don't touch tuple brackets after `in`" by @charliermarsh on 2023-02-23 03:08_

---

_Label `autoformatter` added by @charliermarsh on 2023-02-23 03:08_

---

_Merged by @charliermarsh on 2023-02-23 03:10_

---

_Closed by @charliermarsh on 2023-02-23 03:10_

---

_Branch deleted on 2023-02-23 03:10_

---

_Review comment by @youknowone on `crates/ruff_python_formatter/src/parentheses.rs`:71 on 2023-02-23 03:14_

by using https://github.com/charliermarsh/ruff/pull/3159#discussion_r1115198507,

it can be 

```suggestion
                if !iter.node.is_tuple() {
```
without manually adding methods

---

_@youknowone reviewed on 2023-02-23 03:14_

---
