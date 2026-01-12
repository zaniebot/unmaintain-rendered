```yaml
number: 18196
title: "[ty] Update mypy primer"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: micha/upgrade-mypy-primer
created_at: 2025-05-19T14:52:18Z
updated_at: 2025-05-19T15:35:49Z
url: https://github.com/astral-sh/ruff/pull/18196
synced_at: 2026-01-12T15:56:14Z
```

# [ty] Update mypy primer

---

_@MichaReiser_

## Summary
Pulls in https://github.com/hauntsaninja/mypy_primer/pull/171 which configures `psycopg`'s src root

Thanks @sharkdp for making the change upstream

## Test Plan

CI



---

_Label `ci` added by @MichaReiser on 2025-05-19 14:52_

---

_Comment by @github-actions[bot] on 2025-05-19 14:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MichaReiser on 2025-05-19 14:55_

It's disappointing that we don't see any mypy primer changs but makes sense, given that it mainly tests the binary.

---

_@carljm approved on 2025-05-19 15:08_

Not sure I understand how it's possible that we see no primer diff here, if the mypy-primer change is working correctly, since my understanding is we previously weren't really checking any of the psycopg source code?

But this certainly doesn't seem to do any harm!

---

_Comment by @AlexWaygood on 2025-05-19 15:14_

> Not sure I understand how it's possible that we see no primer diff here, if the mypy-primer change is working correctly, since my understanding is we previously weren't really checking any of the psycopg source code?

It's because we're running the same version of primer on this commit and the commit prior to this commit and then diffing the two sets of diagnostics. Since the same version of primer is being run on both commits, the same psycopg config is being used for both primer runs.

---

_Merged by @MichaReiser on 2025-05-19 15:35_

---

_Closed by @MichaReiser on 2025-05-19 15:35_

---

_Branch deleted on 2025-05-19 15:35_

---
