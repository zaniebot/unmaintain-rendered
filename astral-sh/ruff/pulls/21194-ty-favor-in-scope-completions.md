```yaml
number: 21194
title: "[ty] Favor in scope completions"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: scope-ordering-completions
created_at: 2025-11-01T23:19:04Z
updated_at: 2025-11-06T11:48:50Z
url: https://github.com/astral-sh/ruff/pull/21194
synced_at: 2026-01-12T15:57:18Z
```

# [ty] Favor in scope completions

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/ty/issues/1464

We sort the completions before we add the unimported ones, meaning that imported completions show up before unimported ones.

This is also spoken about in https://github.com/astral-sh/ty/issues/1274, and this is probably a duplicate of that.

@AlexWaygood mentions this [here](https://github.com/astral-sh/ty/issues/1274#issuecomment-3345942698) too.

## Test Plan

Add a test showing even if an unimported completion "should" (alphabetically before) come first, we favor the imported one.


---

_Review requested from @carljm by @MatthewMckee4 on 2025-11-01 23:19_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-11-01 23:19_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-11-01 23:19_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-11-01 23:19_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-11-01 23:19_

---

_Comment by @github-actions[bot] on 2025-11-01 23:21_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-01 23:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-11-01 23:43_

---

_Renamed from "Favor in scope completions" to "[ty] Favor in scope completions" by @AlexWaygood on 2025-11-01 23:43_

---

_Comment by @MatthewMckee4 on 2025-11-02 00:04_

@BurntSushi I have seen that you are planning to change the sorting of completions based on scope anyway, so perhaps this isn't needed

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-02 02:50_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-02 02:50_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-02 02:50_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-11-02 02:50_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-11-02 02:50_

---

_@MichaReiser reviewed on 2025-11-02 02:51_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:362 on 2025-11-02 02:51_

Is it now possible that `dedupe_by` won't deduplicate completions that are available both locally and as import or is it guaranteed that local suggestions and unimported completions never overlap?

---

_@MatthewMckee4 reviewed on 2025-11-02 10:31_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/completion.rs`:362 on 2025-11-02 10:31_

Yes, good point, I'll move that back to the main function

---

_@BurntSushi reviewed on 2025-11-02 13:26_

Thanks!

Would it be simpler to just modify `compare_suggestions` to sort based on whether `Completion::module_name` is present or not? When it's present, that means it's an auto-import suggestion.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:279 on 2025-11-02 14:06_

Delete these extraneous newlines?

---

_Review comment by @BurntSushi on `crates/ty_completion_eval/completion-evaluation-tasks.csv`:26 on 2025-11-02 14:07_

Some of these might worth investigating.

I think we can merge as-is, particularly given the improvement here. But I should probably add snapshots as part of the eval so that changes like this are easier to review.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:895 on 2025-11-02 14:09_

Hmmmm. This is sorting by module name rather than the _existence_ of module name. I think we might just want `completion.module_name.is_none()` here? (That should make the `None` case sort before any `Some` case.)

---

_@BurntSushi requested changes on 2025-11-02 14:09_

---

_@MatthewMckee4 reviewed on 2025-11-02 14:18_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/completion.rs`:895 on 2025-11-02 14:18_

Do we not want `is_some`, so that None < Some (imported < unimported)?

My tests fail if i do `is_none`

---

_Label `server` added by @AlexWaygood on 2025-11-02 15:49_

---

_Review requested from @BurntSushi by @AlexWaygood on 2025-11-03 13:11_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:895 on 2025-11-03 14:32_

Yes!

---

_@BurntSushi reviewed on 2025-11-03 14:32_

---

_@BurntSushi approved on 2025-11-03 14:32_

LGTM, thank you!

---

_Merged by @BurntSushi on 2025-11-03 14:33_

---

_Closed by @BurntSushi on 2025-11-03 14:33_

---

_Branch deleted on 2025-11-06 11:48_

---
