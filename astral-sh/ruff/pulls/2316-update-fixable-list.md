```yaml
number: 2316
title: Update fixable list
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/options
created_at: 2023-01-29T01:18:42Z
updated_at: 2023-01-29T02:03:02Z
url: https://github.com/astral-sh/ruff/pull/2316
synced_at: 2026-01-12T15:55:07Z
```

# Update fixable list

---

_@charliermarsh_

This should really be `ALL`, or code-generated, but I just want to fix this real-quick.

---

_Merged by @charliermarsh on 2023-01-29 01:18_

---

_Closed by @charliermarsh on 2023-01-29 01:18_

---

_Branch deleted on 2023-01-29 01:18_

---

_Comment by @Davrosh on 2023-01-29 01:42_

@charliermarsh Thanks! Looking at it now, C is still listed (as it _is_ probably a substitute for C4 from what I've gathered from the previous conversation), as is COM. C90 is still absent, though, off the top of my head.

---

_Comment by @charliermarsh on 2023-01-29 01:44_

`C` does match `C4` and `C9`. It's admittedly confusing, but because those two categories both have a `C` prefix, they're both matched by `C`. This is different from `COM`, though.

---

_Comment by @Davrosh on 2023-01-29 02:03_

Oh, I see. It's one of those things that only become intuitive after someone explains them. Hopefully this PR fixes the logic, making this discussion redundant 🙏 . Thanks!

---
