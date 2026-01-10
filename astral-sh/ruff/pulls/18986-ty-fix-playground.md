```yaml
number: 18986
title: "[ty] Fix playground"
type: pull_request
state: merged
author: BurntSushi
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: ag/fix-playground
created_at: 2025-06-27T14:26:57Z
updated_at: 2025-06-27T15:16:16Z
url: https://github.com/astral-sh/ruff/pull/18986
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Fix playground

---

_Pull request opened by @BurntSushi on 2025-06-27 14:26_

I renamed a field on a `Completion` struct in #18982, and it looks like
this caused the playground to fail to build:
https://github.com/astral-sh/ruff/actions/runs/15928550050/job/44931734349

Maybe building that playground can be added to CI for pull requests?


---

_Review requested from @MichaReiser by @BurntSushi on 2025-06-27 14:27_

---

_Comment by @MichaReiser on 2025-06-27 14:29_

> Maybe building that playground can be added to CI for pull requests?

It should be part of the CI pipeline... but it's not a required check

---

_Label `playground` added by @MichaReiser on 2025-06-27 14:29_

---

_Label `ty` added by @MichaReiser on 2025-06-27 14:29_

---

_@MichaReiser approved on 2025-06-27 14:29_

---

_Comment by @BurntSushi on 2025-06-27 14:30_

> > Maybe building that playground can be added to CI for pull requests?
> 
> It should be part of the CI pipeline... but it's not a required check

Ah it got skipped: https://github.com/astral-sh/ruff/actions/runs/15928120587/job/44930290023?pr=18982

---

_Merged by @BurntSushi on 2025-06-27 14:43_

---

_Closed by @BurntSushi on 2025-06-27 14:43_

---

_Branch deleted on 2025-06-27 14:43_

---

_Comment by @MichaReiser on 2025-06-27 15:14_

Uhh, we should fix that. We should probably default to always run that job when any code changes

---
