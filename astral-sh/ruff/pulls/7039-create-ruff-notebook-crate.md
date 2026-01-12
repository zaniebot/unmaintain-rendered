```yaml
number: 7039
title: "Create `ruff_notebook` crate"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/jupyter
created_at: 2023-09-01T12:39:52Z
updated_at: 2023-09-01T14:02:57Z
url: https://github.com/astral-sh/ruff/pull/7039
synced_at: 2026-01-12T02:45:38Z
```

# Create `ruff_notebook` crate

---

_Pull request opened by @charliermarsh on 2023-09-01 12:39_

## Summary

This PR moves `ruff/jupyter` into its own `ruff_notebook` crate. Beyond the move itself, there were a few challenges:

1. `ruff_notebook` relies on the source map abstraction. I've moved the source map into `ruff_diagnostics`, since it doesn't have any dependencies on its own and is used alongside diagnostics.
2. `ruff_notebook` has a couple tests for end-to-end linting and autofixing. I had to leave these tests in `ruff` itself.
3. We had code in `ruff/jupyter` that relied on Python lexing, in order to provide a more targeted error message in the event that a user saves a `.py` file with a `.ipynb` extension. I removed this in order to avoid a dependency on the parser, it felt like it wasn't worth retaining just for that dependency.

## Test Plan

`cargo test`


---

_Review requested from @dhruvmanila by @charliermarsh on 2023-09-01 12:39_

---

_Label `internal` added by @charliermarsh on 2023-09-01 12:41_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-01 12:42_

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/source_map.rs`:1 on 2023-09-01 12:56_

I don't have a better recommendation but moving source map to diagnostic feels a bit off because it isn't the only possible use case for source maps. For example, source maps can be used to move the IDE cursor to the right position after applying fixes. However, that requires extracting our fix infrastructure which is rather involved and even `Fix` is defined in `Diagnostic`

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:248 on 2023-09-01 12:58_

Can you explain what this change is about?

---

_@MichaReiser approved on 2023-09-01 12:58_

---

_Comment by @github-actions[bot] on 2023-09-01 13:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@dhruvmanila approved on 2023-09-01 13:11_

That was quick, thanks!

We might want to name it something along the lines of `ruff_notebook` as Jupyter is actually just a frontend while this crate would ideally handle notebooks in general (#6140)

---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/diagnostics.rs`:248 on 2023-09-01 13:14_

I believe it's to merge the `write` and `write_inner` function on `Notebook`. `write` would require a `Path` while `write_inner` would use a `impl Write`. Now, there's only a single public function `write` which takes in a `impl Write`.

The main reason for the separation was in tests where the `write_inner` was being utilized to avoid changing the file on disk and to just use a `Vec` as a writer.

Hope this helps otherwise Charlie can expand as well :)

---

_@dhruvmanila reviewed on 2023-09-01 13:14_

---

_@charliermarsh reviewed on 2023-09-01 13:16_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:248 on 2023-09-01 13:16_

Yeah, I think it's more flexible to expose an interface that takes `impl Write`.

---

_@MichaReiser reviewed on 2023-09-01 13:21_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:248 on 2023-09-01 13:21_

We may want to consider a `&dyn Write` to avoid monomorphizing this for every write implementation.

---

_Renamed from "Create `ruff_jupyter` crate" to "Create `ruff_notebook` crate" by @charliermarsh on 2023-09-01 13:41_

---

_Merged by @charliermarsh on 2023-09-01 13:56_

---

_Closed by @charliermarsh on 2023-09-01 13:56_

---

_Branch deleted on 2023-09-01 13:56_

---
