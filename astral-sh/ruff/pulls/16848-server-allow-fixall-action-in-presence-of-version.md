```yaml
number: 16848
title: "Server: Allow `FixAll` action in presence of version-specific syntax errors"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: codeaction-syntax-errors
created_at: 2025-03-19T15:09:59Z
updated_at: 2025-03-20T10:09:14Z
url: https://github.com/astral-sh/ruff/pull/16848
synced_at: 2026-01-10T19:40:36Z
```

# Server: Allow `FixAll` action in presence of version-specific syntax errors

---

_Pull request opened by @dylwil3 on 2025-03-19 15:09_

The single flag `has_syntax_error` on `LinterResult` is replaced with two (private) flags: `has_valid_syntax` and `has_no_unsupported_syntax_errors`, which record whether there are `ParseError`s or `UnsupportedSyntaxError`s, respectively. Only the former is used to prevent a `FixAll` action.

An attempt has been made to make consistent the usage of the phrases "valid syntax" (which seems to be used to refer only to _parser_ errors) and "syntax error" (which refers to both _parser_ errors and version-specific syntax errors).

Remark for review: `crates/ruff/src/diagnostics.rs` is actually a small diff, not sure what happened there. If you pull it down locally and `git diff --ignore-all-space` it looks better.

Closes #16841

Test:

https://github.com/user-attachments/assets/94f6ed6a-afdc-49f0-8a09-8e2123adb07a





---

_Label `bug` added by @dylwil3 on 2025-03-19 15:09_

---

_Label `server` added by @dylwil3 on 2025-03-19 15:09_

---

_Review requested from @ntBre by @dylwil3 on 2025-03-19 15:09_

---

_Review requested from @dhruvmanila by @dylwil3 on 2025-03-19 15:09_

---

_Comment by @github-actions[bot] on 2025-03-19 15:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-03-19 15:22_

Thanks for jumping on this, LGTM! Just need to propagate the changes to the fuzz crate it looks like.

---

_@ntBre reviewed on 2025-03-19 16:36_

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:634 on 2025-03-19 16:36_

I'm working on adding another type of syntax error now, which drew my attention back to this area. I think we actually want to use `has_no_syntax_errors` (from the first iteration) here to preserve the behavior of the old code instead of recomputing it (on the last iteration). Maybe we need two variables outside the loop now to capture them both from the first iteration?

I think the new errors (semantic/compile-time syntax errors) will be merged into the `has_no_unsupported_syntax_errors` variable to avoid blocking these fixes too.

---

_@dylwil3 reviewed on 2025-03-19 18:09_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/linter.rs`:634 on 2025-03-19 18:09_

Will `has_no_syntax_errors` always be the same as `has_valid_syntax && has_no_unsupported_syntax_errors` and will `has_no_unsupported_syntax_errors` always be the same as `parsed.unsupported_syntax_errors().is_empty()`?

---

_@dylwil3 reviewed on 2025-03-19 18:14_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/linter.rs`:634 on 2025-03-19 18:14_

(In other words - is https://github.com/astral-sh/ruff/pull/16848/commits/a7267ddecd3a3e01b479c58d8e4a99033f96b6b5 the correct interpretation of your suggestion here?)

---

_@ntBre reviewed on 2025-03-19 18:22_

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:634 on 2025-03-19 18:22_

Yeah I think that looks right. It's unfortunate that we can't use the `Parsed` method now, but I guess we really need to differentiate the two bools later instead of grouping them.

I'm thinking the semantic syntax errors will get grouped with the unsupported syntax errors, so I might tweak that name later, but this is exactly what I had in mind for now. Those don't come from the parser, so they won't affect the `Parsed` interface.

I guess we don't have a test for this behavior. I think it would require an intentionally faulty test rule that causes a syntax error.

---

_@dylwil3 reviewed on 2025-03-19 18:31_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/linter.rs`:634 on 2025-03-19 18:31_

Ok perfect, thanks! After this lands you can of course feel free to switch things up as makes sense for the growing menagerie of syntax errors 😄 

> I guess we don't have a test for this behavior. I think it would require an intentionally faulty test rule that causes a syntax error.

That sounds like a great idea! We could have rules that introduce parser errors, version-specific errors, compiler errors, etc.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/linter.rs`:51 on 2025-03-20 10:03_

It's a bit unfortunate that these methods need to be repeated on both `Parsed` and `LinterResult`. We could explore another approach to avoid duplication, not necessarily in this PR, that utilizes `bitflags` for these two booleans like `SyntaxFlags` that can then be queried to ask whether there are parse errors or unsupported syntax errors.

---

_@dhruvmanila approved on 2025-03-20 10:05_

Thanks for the quick fix!

I think we should explore an alternate option to avoid the duplication here as a follow-up.

---

_Comment by @dhruvmanila on 2025-03-20 10:07_

One minor nit: we mainly use the `[...]` syntax for rule categories so I'd avoid using "[server]" and instead just use "Server: " in the PR title.

---

_Renamed from "[`server`]  Allow `FixAll` action in presence of version-specific syntax errors" to "Server: Allow `FixAll` action in presence of version-specific syntax errors" by @dylwil3 on 2025-03-20 10:07_

---

_@dylwil3 reviewed on 2025-03-20 10:08_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/linter.rs`:51 on 2025-03-20 10:08_

Agreed - the current approach seems like it'd be easy for the two to fall out of sync

---

_Merged by @dylwil3 on 2025-03-20 10:09_

---

_Closed by @dylwil3 on 2025-03-20 10:09_

---
