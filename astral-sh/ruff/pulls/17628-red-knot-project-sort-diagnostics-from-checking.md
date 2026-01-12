```yaml
number: 17628
title: "red_knot_project: sort diagnostics from checking files"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: ag/sort-diagnostics
created_at: 2025-04-25T14:15:42Z
updated_at: 2025-04-25T16:38:32Z
url: https://github.com/astral-sh/ruff/pull/17628
synced_at: 2026-01-12T15:56:02Z
```

# red_knot_project: sort diagnostics from checking files

---

_@BurntSushi_

Previously, we could iterate over files in an unspecified order (via
`HashSet` iteration) and we could accumulate diagnostics from files in
an unspecified order (via parallelism).

Here, we change the status quo so that diagnostics collected from files
are sorted after checking is complete. For now, we sort by severity
(with higher severity diagnostics appearing first) and then by
diagnostic ID to give a stable ordering.

I'm not sure if this is the best ordering.

This is meant to address a [flaky test failure](https://github.com/astral-sh/ruff/actions/runs/14641270231/job/41084098266).

---

_Review requested from @carljm by @BurntSushi on 2025-04-25 14:15_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-04-25 14:15_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-04-25 14:15_

---

_Review requested from @sharkdp by @BurntSushi on 2025-04-25 14:15_

---

_Review requested from @dcreager by @BurntSushi on 2025-04-25 14:15_

---

_Label `internal` added by @BurntSushi on 2025-04-25 14:17_

---

_Label `red-knot` added by @BurntSushi on 2025-04-25 14:17_

---

_Label `diagnostics` added by @BurntSushi on 2025-04-25 14:17_

---

_Comment by @github-actions[bot] on 2025-04-25 14:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser reviewed on 2025-04-25 15:30_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/lib.rs`:246 on 2025-04-25 15:30_

I think we should include the file and ranges in the sort order. I would even use them first before comparing against severity and id. It guarantees that diagnostics from the same file stay together and that they preserve source order (because errors further up can result in errors further down). 

What's less clear to me is if the severity should take precedence over the range. For now, I'm leaning to sort by range (similar to `check_file_impl`) first, which I think is also what rustcs does (or rustc doesn't do any sorting?)

---

_@BurntSushi reviewed on 2025-04-25 15:41_

---

_Review comment by @BurntSushi on `crates/red_knot_project/src/lib.rs`:246 on 2025-04-25 15:41_

I don't think rustc does any sorting at this level. It just emits diagnostics as they are found.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-25 15:47_

---

_@MichaReiser reviewed on 2025-04-25 15:48_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/lib.rs`:246 on 2025-04-25 15:48_

This is how Ruff handles this today (I'm a bit confused where it sorts by file but...)

https://github.com/astral-sh/ruff/blob/a192d96880399413432a0316e24ce7982cbc3cb2/crates/ruff/src/commands/check.rs#L157-L171

---

_@ntBre reviewed on 2025-04-25 16:00_

---

_Review comment by @ntBre on `crates/red_knot_project/src/lib.rs`:246 on 2025-04-25 16:00_

I think this is where the file ordering ends up coming from, last time I had to look into this:

https://github.com/astral-sh/ruff/blob/fa88989ef0085610dd8ddecbd289a5fcc1d38892/crates/ruff_linter/src/message/mod.rs#L264-L268

---

_@MichaReiser reviewed on 2025-04-25 16:05_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/lib.rs`:246 on 2025-04-25 16:05_

and here's the file :)

---

_@BurntSushi reviewed on 2025-04-25 16:28_

---

_Review comment by @BurntSushi on `crates/red_knot_project/src/lib.rs`:246 on 2025-04-25 16:28_

Aye. All righty, I've updated the PR to sort by file path, range, severity and ID. In that order.

---

_@MichaReiser approved on 2025-04-25 16:32_

Thanks

---

_Merged by @BurntSushi on 2025-04-25 16:38_

---

_Closed by @BurntSushi on 2025-04-25 16:38_

---

_Branch deleted on 2025-04-25 16:38_

---
