```yaml
number: 5891
title: Skip git tests on Windows
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/windows-git
created_at: 2024-08-07T20:37:12Z
updated_at: 2024-08-08T15:37:26Z
url: https://github.com/astral-sh/uv/pull/5891
synced_at: 2026-01-10T13:31:54Z
```

# Skip git tests on Windows

---

_Pull request opened by @zanieb on 2024-08-07 20:37_

Might be pushing it on test coverage, but these are some of our slowest tests we might get a significant speedup here.

Part of #5713 

---

_Label `testing` added by @zanieb on 2024-08-07 20:37_

---

_Comment by @zanieb on 2024-08-07 20:44_

From 5m 47s to 4m 47s for the `cargo test` step.

---

_Comment by @zanieb on 2024-08-07 21:34_

Happy to hear thoughts on this, I don't know how nuanced the git behavior is on Windows specifically. We could adjust the features on `main` vs pull requests too.

---

_Marked ready for review by @zanieb on 2024-08-07 21:34_

---

_Comment by @charliermarsh on 2024-08-07 21:56_

I think I'd leave this as-is for now given those numbers. Though it's possible we're not using this feature everywhere we should.

---

_Comment by @zanieb on 2024-08-08 15:12_

With #5910 we go from 6m 12s (7m 42s total) to 4m 41s (5m 57s total) for a  25% (22% total) improvement.

---

_Comment by @zanieb on 2024-08-08 15:14_

(Previously it was about 17%)

---

_@charliermarsh approved on 2024-08-08 15:26_

---

_Comment by @charliermarsh on 2024-08-08 15:26_

I'm cool with it, I think.

---

_Comment by @zanieb on 2024-08-08 15:37_

If we have a regression it's easy to revert!

---

_Merged by @zanieb on 2024-08-08 15:37_

---

_Closed by @zanieb on 2024-08-08 15:37_

---

_Branch deleted on 2024-08-08 15:37_

---
