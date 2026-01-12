```yaml
number: 1701
title: "Include list of fixed files in `stderr` output"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/show-files
created_at: 2023-01-07T00:08:23Z
updated_at: 2023-01-07T05:31:58Z
url: https://github.com/astral-sh/ruff/pull/1701
synced_at: 2026-01-12T15:55:06Z
```

# Include list of fixed files in `stderr` output

---

_@charliermarsh_

This PR adds the list of fixed files to Ruff's output (send to `stderr`, and omitted for `--silent` and `--quiet` settings):

![Screen Shot 2023-01-06 at 7 07 57 PM](https://user-images.githubusercontent.com/1309177/211120323-6203e624-b38d-47ae-a544-8d13e2410396.png)

Closes #1667.

---

_Review comment by @andersk on `src/linter.rs`:218 on 2023-01-07 00:14_

Are we going with “violation(s)” here too?

---

_@andersk reviewed on 2023-01-07 00:14_

---

_@charliermarsh reviewed on 2023-01-07 00:18_

---

_Review comment by @charliermarsh on `src/linter.rs`:218 on 2023-01-07 00:18_

Eventually, but for now I wanted it to be consistent with the rest of the output (which says "error(s)" at the bottom).

Also: we may continue to say "error" in both places, because an error will be a severity level for a violation (so, in theory, we could say "X errors and Y warnings").

---

_Merged by @charliermarsh on 2023-01-07 00:51_

---

_Closed by @charliermarsh on 2023-01-07 00:51_

---

_Branch deleted on 2023-01-07 00:51_

---

_Comment by @charliermarsh on 2023-01-07 05:31_

@smackesey - Not urgent but just looking for some user feedback -- does this seem useful, or too noisy? (I reverted this change for now so it won't go out yet.)

---
