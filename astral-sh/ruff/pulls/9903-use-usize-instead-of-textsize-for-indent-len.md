```yaml
number: 9903
title: "Use `usize` instead of `TextSize` for `indent_len`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: docstring-indent-u32
created_at: 2024-02-08T23:01:10Z
updated_at: 2024-02-09T20:41:37Z
url: https://github.com/astral-sh/ruff/pull/9903
synced_at: 2026-01-10T22:57:09Z
```

# Use `usize` instead of `TextSize` for `indent_len`

---

_Pull request opened by @MichaReiser on 2024-02-08 23:01_

Using `TextSize` as the return type for `indent_len` is problematic because it doesn't represent a byte offset but the indent's number of columns.

This PR changes the return type to `usize` to clarify that the `indentation_length`'s returned value **is not** a byte offset.


## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-02-08 23:01_

---

_Review requested from @BurntSushi by @MichaReiser on 2024-02-08 23:01_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/string/docstring.rs`:1544 on 2024-02-09 02:15_

If this is meant to be a byte offset, then I think this should be a `usize`. And then you can get rid of the `as usize` conversions above.

But if it's a byte offset, then what about the tab business going on in here? I think that makes this not a byte offset but maybe something else?

Clearer docs on this function might help.

---

_@BurntSushi reviewed on 2024-02-09 02:15_

---

_Renamed from "Use `u32` instead of `TextSize` for `indent_len`" to "Use `usize` instead of `TextSize` for `indent_len`" by @MichaReiser on 2024-02-09 02:35_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/docstring.rs`:1544 on 2024-02-09 02:38_

Yes, good point. I updated it to use `usize`. It also avoids the issue that there's no `u32` to `usize` conversion. 

The point of this PR is that `indentation_length` **does not** return a byte offset. It's the length/width in columns (or spaces). That's why we shouldn't use `TextSize` or `TextRange`, which we exclusively use for byte offsets. 

---

_@MichaReiser reviewed on 2024-02-09 02:38_

---

_Comment by @github-actions[bot] on 2024-02-09 02:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @MichaReiser on 2024-02-09 20:41_

---

_Closed by @MichaReiser on 2024-02-09 20:41_

---

_Branch deleted on 2024-02-09 20:41_

---
