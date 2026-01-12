```yaml
number: 53
title: "perf: Improve search for duplicates"
type: pull_request
state: merged
author: Stranger6667
labels: []
assignees: []
merged: true
base: main
head: dd/avoid-clone
created_at: 2022-08-31T08:02:34Z
updated_at: 2022-08-31T12:26:24Z
url: https://github.com/astral-sh/ruff/pull/53
synced_at: 2026-01-12T15:55:04Z
```

# perf: Improve search for duplicates

---

_@Stranger6667_

Hi!

First of all, thank you for building this project! :) It is absolutely stunning to see such tooling being developed :)

This PR improves performance in the search for duplicates by avoiding cloning a `String` - a string slice is sufficient.

---

_Comment by @charliermarsh on 2022-08-31 12:18_

Thank you, that's so nice of you to say :) Appreciate the change!

---

_Merged by @charliermarsh on 2022-08-31 12:23_

---

_Closed by @charliermarsh on 2022-08-31 12:23_

---

_Branch deleted on 2022-08-31 12:26_

---
