```yaml
number: 3212
title: More enum work
type: pull_request
state: merged
author: youknowone
labels: []
assignees: []
merged: true
base: main
head: enum-work
created_at: 2023-02-24T18:27:24Z
updated_at: 2023-02-25T16:40:17Z
url: https://github.com/astral-sh/ruff/pull/3212
synced_at: 2026-01-12T04:39:44Z
```

# More enum work

---

_Pull request opened by @youknowone on 2023-02-24 18:27_

_No description provided._

---

_Review comment by @youknowone on `crates/ruff_python_formatter/src/builders.rs`:40 on 2023-02-24 18:29_

This is up to preference, but if you want, these changes are also possible.

```suggestion
                if let Some(range) = comment.kind.own_line_comment() {
```

---

_@youknowone reviewed on 2023-02-24 18:29_

---

_Review comment by @not-my-profile on `crates/ruff_cli/src/main.rs`:208 on 2023-02-24 19:55_

I actually consider this to be less readable. This now looks like `autofix` is an `Option`.

---

_@not-my-profile reviewed on 2023-02-24 19:55_

---

_@charliermarsh reviewed on 2023-02-24 20:18_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/main.rs`:208 on 2023-02-24 20:18_

I guess `None` is arguably not a great enum variant name anyway?

---

_@charliermarsh reviewed on 2023-02-24 20:18_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/builders.rs`:40 on 2023-02-24 20:18_

I prefer what we have now.

---

_@youknowone reviewed on 2023-02-25 01:51_

---

_Review comment by @youknowone on `crates/ruff_cli/src/main.rs`:208 on 2023-02-25 01:51_

I removed this commit

---

_Review comment by @not-my-profile on `crates/ruff_cli/src/main.rs`:208 on 2023-02-25 11:34_

Thanks. I don't think there's any problem with having `None` as an enum variant name.

---

_@not-my-profile reviewed on 2023-02-25 11:34_

---

_Merged by @charliermarsh on 2023-02-25 16:40_

---

_Closed by @charliermarsh on 2023-02-25 16:40_

---
