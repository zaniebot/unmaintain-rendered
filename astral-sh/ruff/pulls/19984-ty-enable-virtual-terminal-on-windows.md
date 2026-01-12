```yaml
number: 19984
title: "[ty] Enable virtual terminal on Windows"
type: pull_request
state: merged
author: sharkdp
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: david/fix-1045
created_at: 2025-08-19T07:46:32Z
updated_at: 2025-08-19T09:14:06Z
url: https://github.com/astral-sh/ruff/pull/19984
synced_at: 2026-01-12T15:56:51Z
```

# [ty] Enable virtual terminal on Windows

---

_@sharkdp_

## Summary

I believe this fixes https://github.com/astral-sh/ty/issues/1045

## Test Plan

I have not tested this, but I use this in some of my other projects.

---

_Review requested from @carljm by @sharkdp on 2025-08-19 07:46_

---

_Review requested from @MichaReiser by @sharkdp on 2025-08-19 07:46_

---

_Review requested from @dcreager by @sharkdp on 2025-08-19 07:46_

---

_Label `ty` added by @sharkdp on 2025-08-19 07:46_

---

_Label `bug` added by @sharkdp on 2025-08-19 07:46_

---

_Comment by @github-actions[bot] on 2025-08-19 07:48_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-19 07:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @sharkdp on `crates/ty/src/lib.rs`:66 on 2025-08-19 07:52_

`set_virtual_terminal` [will always return `Ok(())`](https://github.com/colored-rs/colored/blob/68761c1dfe306c870aa94af085c4686bce8d5fbd/src/control.rs#L14-L15).

---

_@sharkdp reviewed on 2025-08-19 07:52_

---

_@MichaReiser reviewed on 2025-08-19 08:59_

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:66 on 2025-08-19 08:59_

Maybe add a comment similar to ruff:

```
    // Enabled ANSI colors on Windows 10.
    #[cfg(windows)]
    assert!(colored::control::set_virtual_terminal(true).is_ok());
```

---

_@MichaReiser approved on 2025-08-19 08:59_

---

_Comment by @MichaReiser on 2025-08-19 08:59_

Thank you. 

---

_@sharkdp reviewed on 2025-08-19 09:10_

---

_Review comment by @sharkdp on `crates/ty/src/lib.rs`:66 on 2025-08-19 09:10_

Oh, oops. I even searched for `set_virtual_terminal` and was surprised we didn't use it anywhere. Forgot to disable my `crates/ty*` filter ...

Made it consistent with what we have in ruff.

---

_Merged by @sharkdp on 2025-08-19 09:13_

---

_Closed by @sharkdp on 2025-08-19 09:13_

---

_Branch deleted on 2025-08-19 09:13_

---
