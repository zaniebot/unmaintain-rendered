```yaml
number: 17640
title: "[red-knot] Use 101 exit code when there's at least one diagnostic with severity 'fatal'"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/fatal-exit-with-error
created_at: 2025-04-26T09:56:45Z
updated_at: 2025-04-28T08:03:16Z
url: https://github.com/astral-sh/ruff/pull/17640
synced_at: 2026-01-10T19:03:00Z
```

# [red-knot] Use 101 exit code when there's at least one diagnostic with severity 'fatal'

---

_Pull request opened by @MichaReiser on 2025-04-26 09:56_

## Summary

We now capture panics during checking and emit a diagnostic with severity `Fatal` but the CLI
still exits with `ExitStatus::Failure` (1) which makes it indistinguishable from a normal: there are diagnostics. 

This PR changes the implementation to return `ExitStatus::Eror` (which we code to 2)

## Test Plan

I ran red knot on a project where it panics and printed the exit code:

```bash
✦35 ❯ echo $status
2
```


---

_Review requested from @carljm by @MichaReiser on 2025-04-26 09:56_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-26 09:56_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-26 09:56_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-26 09:56_

---

_Label `red-knot` added by @MichaReiser on 2025-04-26 09:56_

---

_Comment by @github-actions[bot] on 2025-04-26 09:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@sharkdp approved on 2025-04-28 07:17_

Are panics the only source of "fatal" severity? If so, could we exit with a code other than 1 or 2? That would make panics easy to detect.

---

_Comment by @MichaReiser on 2025-04-28 07:27_

> Are panics the only source of "fatal" severity? If so, could we exit with a code other than 1 or 2? That would make panics easy to detect.

Not strictly. But the `fatal` severity is reserved for internal errors (not project/user errors). What error code would you suggest? I did search for some reference on error codes and couldn't find any recommended error code other than 2. 


---

_Comment by @AlexWaygood on 2025-04-28 07:48_

I don't know if this is in line with some convention or a black-specific practice, but black always exits with code 123 if there's an internal error https://black.readthedocs.io/en/stable/usage_and_configuration/the_basics.html#check

---

_Comment by @sharkdp on 2025-04-28 07:51_

Rust programs exit with [code 101](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=ccc5d0b27219672b8498b1569daac58d) for panics. I think it would be reasonable to do the same, unless the "fatal" category also includes other sources of errors.

---

_Renamed from "[red-knot] Use 'error' exit code when there's at least one diagnostic with severity 'fatal'" to "[red-knot] Use 101 exit code when there's at least one diagnostic with severity 'fatal'" by @MichaReiser on 2025-04-28 07:56_

---

_Comment by @MichaReiser on 2025-04-28 08:02_

I don't think there's any good error code here. E.g. cargo uses 101 for any compilation error (e.g. because the input program is invalid). 

I'll go with 101 for now.

---

_Merged by @MichaReiser on 2025-04-28 08:03_

---

_Closed by @MichaReiser on 2025-04-28 08:03_

---

_Branch deleted on 2025-04-28 08:03_

---
