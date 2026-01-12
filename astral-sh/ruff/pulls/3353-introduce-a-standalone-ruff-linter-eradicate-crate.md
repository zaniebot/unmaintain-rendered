```yaml
number: 3353
title: "Introduce a standalone `ruff_linter_eradicate` crate"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/crate
created_at: 2023-03-05T21:45:34Z
updated_at: 2023-03-18T02:23:21Z
url: https://github.com/astral-sh/ruff/pull/3353
synced_at: 2026-01-12T15:55:12Z
```

# Introduce a standalone `ruff_linter_eradicate` crate

---

_@charliermarsh_

Trying to push to the point at which this is actually possible, to figure out what breaks.

---

_@charliermarsh reviewed on 2023-03-05 21:46_

---

_Review comment by @charliermarsh on `crates/ruff_linter_eradicate/src/detection.rs`:103 on 2023-03-05 21:46_

We may not be able to move the snapshot tests into the standalone crates, since they depend on Settings _and_ they depend on all the core linter logic.

---

_@charliermarsh reviewed on 2023-03-05 21:47_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/fix.rs`:2 on 2023-03-05 21:47_

(These files would be moved from `crates/ruff`, rather than copied.)

---

_@charliermarsh reviewed on 2023-03-05 21:47_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/locator.rs`:3 on 2023-03-05 21:47_

(These files would be moved from `crates/ruff`, rather than copied. I think this roughly matches @MichaReiser's suggestion from the "split up crates" issue.)

---

_Renamed from "Introduce a standalone ruff_linter_eradicate crate" to "Introduce a standalone `ruff_linter_eradicate` crate" by @charliermarsh on 2023-03-05 21:47_

---

_Comment by @charliermarsh on 2023-03-06 23:37_

This isn't worth reviewing yet, but I think with #3352 and #3370 we could actually start moving rules out into separate crates if we choose to do so. Settings and tests are unsolved, but those would likely remain back in `ruff`.

---

_Comment by @charliermarsh on 2023-03-09 00:54_

Okay, this is fully working now, though settings (\cc @MichaReiser) are a bit awkward.

---

_Closed by @charliermarsh on 2023-03-18 02:23_

---

_Branch deleted on 2023-03-18 02:23_

---

_Comment by @charliermarsh on 2023-03-18 02:23_

Undecided whether we actually _want_ to do this but it's now possible.

---
