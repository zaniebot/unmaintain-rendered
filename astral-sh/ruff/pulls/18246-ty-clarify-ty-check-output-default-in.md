```yaml
number: 18246
title: "[ty] Clarify `ty check` output default in documentation."
type: pull_request
state: merged
author: brainwane
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: cli-ty
created_at: 2025-05-21T20:50:20Z
updated_at: 2025-05-22T15:44:32Z
url: https://github.com/astral-sh/ruff/pull/18246
synced_at: 2026-01-12T15:56:15Z
```

# [ty] Clarify `ty check` output default in documentation.

---

_@brainwane_

## Summary

Add a "[default]" note in the CLI documentation to indicate that, by default, `ty check` prints "full" diagnostic output, not "concise" output.

Was: https://github.com/astral-sh/ty/pull/478 .

## Test Plan

I pushed the branch, and read the preview of the Markdown file on GitHub in my browser.

---

_Review requested from @carljm by @brainwane on 2025-05-21 20:50_

---

_Review requested from @MichaReiser by @brainwane on 2025-05-21 20:50_

---

_Review requested from @AlexWaygood by @brainwane on 2025-05-21 20:50_

---

_Review requested from @sharkdp by @brainwane on 2025-05-21 20:50_

---

_Review requested from @dcreager by @brainwane on 2025-05-21 20:50_

---

_Comment by @github-actions[bot] on 2025-05-21 20:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @carljm on 2025-05-21 20:59_

@MichaReiser This seems like useful information for users to have in the command reference; should we just add it in the comment at https://github.com/astral-sh/ruff/blob/d098118e37be9437345d527c29a5aeea91b676a6/crates/ty/src/args.rs#L289, or is there a way to update the script so that we automatically output this in the reference based on the existing `[#default]` annotation?

---

_Label `documentation` added by @AlexWaygood on 2025-05-21 22:03_

---

_Comment by @MichaReiser on 2025-05-22 05:48_

> , or is there a way to update the script so that we automatically output this in the reference based on the existing [#default] annotation?

This seems hard because it requires understanding Rust code. But yes, we should update the docs comment in `args.rs` and then run `cargo dev generate-all` to generate the docs.

---

_Converted to draft by @brainwane on 2025-05-22 09:49_

---

_Label `ty` added by @MichaReiser on 2025-05-22 13:15_

---

_Comment by @MichaReiser on 2025-05-22 13:24_

Thanks

---

_Marked ready for review by @MichaReiser on 2025-05-22 13:24_

---

_Merged by @MichaReiser on 2025-05-22 13:24_

---

_Closed by @MichaReiser on 2025-05-22 13:24_

---

_Branch deleted on 2025-05-22 15:44_

---
