```yaml
number: 21146
title: "[ty] Don't provide completions when in class or function definition"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - bug
  - server
  - ty
assignees: []
merged: true
base: main
head: no-completions-in-definitions
created_at: 2025-10-30T16:50:17Z
updated_at: 2025-11-01T15:07:32Z
url: https://github.com/astral-sh/ruff/pull/21146
synced_at: 2026-01-12T15:57:17Z
```

# [ty] Don't provide completions when in class or function definition

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

Resolves https://github.com/astral-sh/ty/issues/1457

We now check if we are in a situation where we are in the name of class or function definition like `def f` or `class f`. We check if the second last token is `def` or `class`.

## Test Plan

Added test shown in that issue.

Also add test for situation where we have not started typing the name like `def ` and `class `. My changes do not change any behavior there, but i think it is good to show we still dont provide completions there


---

_Review requested from @carljm by @MatthewMckee4 on 2025-10-30 16:50_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-10-30 16:50_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-10-30 16:50_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-10-30 16:50_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-10-30 16:50_

---

_Label `bug` added by @AlexWaygood on 2025-10-30 16:50_

---

_Label `server` added by @AlexWaygood on 2025-10-30 16:50_

---

_Label `ty` added by @AlexWaygood on 2025-10-30 16:50_

---

_Comment by @github-actions[bot] on 2025-10-30 16:52_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Renamed from "No completions in definitions" to "[ty] Don't provide completions when in class or function definition" by @MatthewMckee4 on 2025-10-30 16:53_

---

_Comment by @github-actions[bot] on 2025-10-30 16:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review request for @dcreager removed by @MichaReiser on 2025-10-30 19:41_

---

_Review request for @carljm removed by @MichaReiser on 2025-10-30 19:41_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-10-30 19:41_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-10-30 19:41_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-10-30 19:41_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:218 on 2025-10-30 19:43_

Should we pull out the `tokens.before()`? It seems every one of those functions now searches the tokens before `offset`.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:866 on 2025-10-30 19:44_

I forgot to mention this in my PR but we should probably do this too in `type <CURSOR>. 

---

_@MichaReiser approved on 2025-10-30 19:44_

Thank you

---

_@MatthewMckee4 reviewed on 2025-10-30 20:03_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/completion.rs`:218 on 2025-10-30 20:03_

Good idea

---

_@MatthewMckee4 reviewed on 2025-10-30 20:27_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/completion.rs`:866 on 2025-10-30 20:27_

cool, will cover that, though i think there is a case where we can't catch it:

<img width="970" height="400" alt="image" src="https://github.com/user-attachments/assets/1e0fd14d-500c-47cb-a6c6-acbc8c02d39f" />

`type` here is just `Name` in the ast.

What we can catch is this:

```py
type f<CURSOR> = int
```

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/completion.rs`:866 on 2025-10-30 20:28_

(unless i look directly at the string, but I'm not sure if this is right or not.

---

_@MatthewMckee4 reviewed on 2025-10-30 20:28_

---

_@MichaReiser reviewed on 2025-10-30 21:37_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:866 on 2025-10-30 21:37_

Oh right, because it is a contextual keyword. I think inspecting the source would be fine here because I don't see any case where `type <name>` is valid. 

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/completion.rs`:866 on 2025-10-30 22:07_

This does not seem like the right approach, but i can't seem to find any other apis to use to do this.

---

_@MatthewMckee4 reviewed on 2025-10-30 22:07_

---

_@MichaReiser reviewed on 2025-10-30 22:30_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:866 on 2025-10-30 22:30_

You want something like:

```rust
if matches!(t.kind(), TokenKind::Def | TokenKind::Class | TokenKind::Type) {
	true
} else if t.kind() == TokenKind::Name {
	let source = source_text(db, file);
	
 	&source[t.range()] == "type"
} else {
	false 
}
```

---

_@MatthewMckee4 reviewed on 2025-10-30 22:32_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/completion.rs`:866 on 2025-10-30 22:32_

cool, thanks

---

_@MichaReiser approved on 2025-10-30 23:19_

Sweet

---

_Merged by @MichaReiser on 2025-10-30 23:19_

---

_Closed by @MichaReiser on 2025-10-30 23:19_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:843 on 2025-10-31 13:00_

The doc comments for this function (and the one above) should be updated.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:4160 on 2025-10-31 13:03_

I don't think these tests need to be enabling `auto_import()`. That's a bit of a big hammer. You can instead add a local item in the source file and test completions on that.

---

_@BurntSushi reviewed on 2025-10-31 13:03_

I realize this PR was already merged, but I wanted to leave feedback anyway.

I can address these in a follow-up PR if you don't want though. :-)

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:855 on 2025-10-31 13:06_

I think it would be better to not repeat the implementation in the doc comment where possible. e.g., Your doc comment doesn't include `type`. Instead, something like, "Returns true when the tokens indicate that the definition of a new name is being introduced at the end."

---

_@BurntSushi reviewed on 2025-10-31 13:06_

---

_Comment by @MatthewMckee4 on 2025-11-01 14:54_

Thanks for the feedback, ill address this in a PR

---

_Branch deleted on 2025-11-01 15:07_

---
