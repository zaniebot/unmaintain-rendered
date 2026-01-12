```yaml
number: 8055
title: "Accept `--target-version` in the format CLI"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - formatter
assignees: []
merged: true
base: main
head: charlie/target-version
created_at: 2023-10-18T20:44:02Z
updated_at: 2023-10-19T21:30:14Z
url: https://github.com/astral-sh/ruff/pull/8055
synced_at: 2026-01-12T15:55:25Z
```

# Accept `--target-version` in the format CLI

---

_@charliermarsh_

## Summary

This doesn't affect behavior _yet_ (see: https://github.com/astral-sh/ruff/issues/7234), but it will be needed in the future, and it's surprising to users that it doesn't exist.

Closes https://github.com/astral-sh/ruff/issues/8051.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-10-18 20:44_

---

_Review requested from @konstin by @charliermarsh on 2023-10-18 20:44_

---

_Label `cli` added by @charliermarsh on 2023-10-18 20:44_

---

_Label `formatter` added by @charliermarsh on 2023-10-18 20:44_

---

_Comment by @github-actions[bot] on 2023-10-18 21:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @MichaReiser on 2023-10-18 21:54_

I'm conflicted on this. I find it weird to have no-op flags. 

---

_Comment by @charliermarsh on 2023-10-18 22:02_

I think we should include it. It’s sort of “incidentally” a no-op. It _does_ set the target version, we’re just not using it in any formatting decisions yet. But we will soon (for preview style, at least).

---

_Comment by @MichaReiser on 2023-10-18 23:17_

Yeah. But it's a no-op feature that you can't test. 

---

_@MichaReiser approved on 2023-10-18 23:18_

---

_Merged by @charliermarsh on 2023-10-19 00:14_

---

_Closed by @charliermarsh on 2023-10-19 00:14_

---

_Branch deleted on 2023-10-19 00:14_

---

_Comment by @charliermarsh on 2023-10-19 00:14_

So it must be working...

---

_Comment by @henryiii on 2023-10-19 21:27_

Does this also get picked up from the standard python-requires, like the target-version for linting (and black does this too)?

---

_Comment by @charliermarsh on 2023-10-19 21:30_

Yup, should be, but I will confirm.

---
