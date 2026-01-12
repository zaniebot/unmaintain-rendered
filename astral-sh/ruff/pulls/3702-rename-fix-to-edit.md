```yaml
number: 3702
title: "Rename `Fix` to `Edit`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/edit
created_at: 2023-03-23T23:41:39Z
updated_at: 2023-03-24T23:29:16Z
url: https://github.com/astral-sh/ruff/pull/3702
synced_at: 2026-01-12T15:55:13Z
```

# Rename `Fix` to `Edit`

---

_@charliermarsh_

## Summary

I want to extend the autofix API to support multiple edits, similar to the LSP `WorkspaceEdit` API. (This is also necessary to support, e.g., inserting required imports for relevant fixes.) As part of that change, I'm proposing that we rename `Fix` to `Edit`, such that a `Fix` can in the future be modeled as a collection of edits.

This PR is a straight rename of the struct with no functional changes.


---

_Comment by @github-actions[bot] on 2023-03-23 23:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     19.5±0.97ms     2.1 MB/sec    1.00     19.0±0.90ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.07      4.9±0.24ms     3.4 MB/sec    1.00      4.6±0.20ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   661.6±24.77µs     4.5 MB/sec    1.00   663.0±25.88µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.1±0.35ms     3.1 MB/sec    1.07      8.7±0.52ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.06     11.5±0.65ms     3.5 MB/sec    1.00     10.8±0.65ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.3±0.11ms     7.2 MB/sec    1.00      2.2±0.08ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   259.2±12.80µs    11.4 MB/sec    1.04   270.1±11.15µs    10.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      5.2±0.17ms     4.9 MB/sec    1.00      5.1±0.27ms     5.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.3±0.14ms     2.7 MB/sec    1.00     15.3±0.19ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.2±0.03ms     4.0 MB/sec    1.00      4.1±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    461.1±6.40µs     6.4 MB/sec    1.00   457.8±11.01µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.8±0.10ms     3.7 MB/sec    1.00      6.7±0.03ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.03      8.4±0.06ms     4.9 MB/sec    1.00      8.2±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1804.3±19.11µs     9.2 MB/sec    1.00  1754.5±15.28µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    186.0±2.17µs    15.9 MB/sec    1.00    184.0±6.83µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.9±0.05ms     6.6 MB/sec    1.00      3.8±0.03ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @JonathanPlasse on 2023-03-24 00:03_

- This will conflict with #3701, in which order should we merge them?

---

_Renamed from "Rename Fix to Edit" to "Rename `Fix` to `Edit`" by @charliermarsh on 2023-03-24 02:54_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-24 02:54_

---

_Marked ready for review by @charliermarsh on 2023-03-24 02:54_

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/diagnostic.rs`:30 on 2023-03-24 07:47_

Is my understanding correct that you plan to replace `Option<Edit>` with `Option<Fix>` in a follow up PR (and thus, keeping the name `fix` makes sense).

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/edit.rs`:7 on 2023-03-24 07:47_

Do you want to use this change as an opportunity to document the struct?

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/edit.rs`:17 on 2023-03-24 07:51_

Unrelated to this PR but I wonder if it would be more appropriate to use an `enum` instead

```rust
enum Edit {
  Deletion { start: Location, end: Location },
  Insertion { text: String, at: Location },
  Replacement { text: String, start: Location, end: Location }
}
```

Or

```rust
struct Edit {
	kind: EditKind,
	at: Location
}

enum EditKind {
	Deletion { end: Location },
	Insertion { text: String },
	Replacement { with: String, end: Location }
}
```



---

_@MichaReiser approved on 2023-03-24 07:52_

---

_@charliermarsh reviewed on 2023-03-24 21:23_

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/diagnostic.rs`:30 on 2023-03-24 21:23_

Yes!

---

_@charliermarsh reviewed on 2023-03-24 21:23_

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/edit.rs`:7 on 2023-03-24 21:23_

Why would we want docs?

---

_@MichaReiser reviewed on 2023-03-24 21:28_

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/edit.rs`:7 on 2023-03-24 21:28_

🧐

---

_@charliermarsh reviewed on 2023-03-24 21:29_

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/edit.rs`:7 on 2023-03-24 21:29_

:joy:

---

_Merged by @charliermarsh on 2023-03-24 23:29_

---

_Closed by @charliermarsh on 2023-03-24 23:29_

---

_Branch deleted on 2023-03-24 23:29_

---
