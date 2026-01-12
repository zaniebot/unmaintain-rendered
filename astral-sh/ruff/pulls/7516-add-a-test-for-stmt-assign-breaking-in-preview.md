```yaml
number: 7516
title: Add a test for stmt assign breaking in preview mode
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: test-for-breaking-in-preview-mode
created_at: 2023-09-19T13:15:40Z
updated_at: 2023-09-19T14:16:42Z
url: https://github.com/astral-sh/ruff/pull/7516
synced_at: 2026-01-12T02:39:10Z
```

# Add a test for stmt assign breaking in preview mode

---

_Pull request opened by @konstin on 2023-09-19 13:15_

In preview mode, black will consistently break the right side first. This doesn't work yet, but we'll need the test later.

---

_Label `internal` added by @konstin on 2023-09-19 13:15_

---

_Label `formatter` added by @konstin on 2023-09-19 13:15_

---

_@MichaReiser approved on 2023-09-19 13:36_

Are you planning on this? I'm asking because it feels a bit strange to have some preview formatting test cases in the code base, which may even be confusing, without us working on it otherwise.

To be clear, I don't mind merging this. It's just a somewhat unexpected change.

---

_Comment by @konstin on 2023-09-19 14:16_

i don't plan to work on this atm (there are other priorities than preview mode). i lost track of the state of this and these changes make it easy to look them up and also prevent regressions in either style

---

_Merged by @konstin on 2023-09-19 14:16_

---

_Closed by @konstin on 2023-09-19 14:16_

---

_Branch deleted on 2023-09-19 14:16_

---
