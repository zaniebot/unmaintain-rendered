```yaml
number: 8066
title: Update tutorial to match revised Ruff defaults
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/tutorial
created_at: 2023-10-19T15:59:09Z
updated_at: 2023-10-19T16:33:16Z
url: https://github.com/astral-sh/ruff/pull/8066
synced_at: 2026-01-12T02:32:41Z
```

# Update tutorial to match revised Ruff defaults

---

_Pull request opened by @charliermarsh on 2023-10-19 15:59_

## Summary

We don't enable E501 by default, but `line-length` is a useful example for configuration, so we now set `--extend-select` in the tutorial with a note to that effect.

I've also updated all the outputs to match the latest CLI behavior, and changed the example from `List` to `Sequence` because `List` now spits out two diagnostics (one for the import, one for the usage), which IMO is confusing for beginners.

---

_Review requested from @zanieb by @charliermarsh on 2023-10-19 15:59_

---

_Label `documentation` added by @charliermarsh on 2023-10-19 15:59_

---

_Review comment by @zanieb on `README.md`:241 on 2023-10-19 16:08_

perhaps "overlap" instead of "conflict"?

---

_@zanieb reviewed on 2023-10-19 16:08_

---

_@zanieb reviewed on 2023-10-19 16:08_

---

_Review comment by @zanieb on `docs/tutorial.md`:19 on 2023-10-19 16:08_

Not really important for this to be a sequence is it? Wouldn't `Iterable` be proper?

---

_@zanieb approved on 2023-10-19 16:09_

---

_@zanieb reviewed on 2023-10-19 16:09_

---

_Review comment by @zanieb on `docs/tutorial.md`:19 on 2023-10-19 16:09_

Oh sorry I see you changed this to avoid additional diagnostics — it's fine as `Sequence`

---

_Review comment by @charliermarsh on `docs/tutorial.md`:19 on 2023-10-19 16:12_

Thanks!

---

_@charliermarsh reviewed on 2023-10-19 16:12_

---

_@charliermarsh reviewed on 2023-10-19 16:13_

---

_Review comment by @charliermarsh on `docs/tutorial.md`:19 on 2023-10-19 16:13_

`Iterable` works just as well.

---

_Merged by @charliermarsh on 2023-10-19 16:27_

---

_Closed by @charliermarsh on 2023-10-19 16:27_

---

_Branch deleted on 2023-10-19 16:27_

---

_Comment by @github-actions[bot] on 2023-10-19 16:33_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
