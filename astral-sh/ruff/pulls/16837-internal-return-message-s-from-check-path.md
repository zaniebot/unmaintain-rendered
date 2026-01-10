```yaml
number: 16837
title: "[internal] Return `Message`s from `check_path`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/check-path-messages
created_at: 2025-03-18T21:03:01Z
updated_at: 2025-03-19T14:08:11Z
url: https://github.com/astral-sh/ruff/pull/16837
synced_at: 2026-01-10T19:40:36Z
```

# [internal] Return `Message`s from `check_path`

---

_Pull request opened by @ntBre on 2025-03-18 21:03_

Summary
--

This PR updates `check_path` in the `ruff_linter` crate to return a `Vec<Message>` instead of a `Vec<Diagnostic>`. The main motivation for this is to make it easier to convert semantic syntax errors directly into `Message`s rather than `Diagnostic`s in #16106. However, this also has the benefit of keeping the preview check on unsupported syntax errors in `check_path`, as suggested in https://github.com/astral-sh/ruff/pull/16429#discussion_r1974748024.

All of the interesting changes are in the first commit. The second commit just renames variables like `diagnostics` to `messages`, and the third commit is a tiny import fix.

I also updated the `ExpandedMessage::location` field name, which caused a few extra commits tidying up the playground code. I thought it was nicely symmetric with `end_location`, but I'm happy to revert that too.

Test Plan
--

Existing tests. I also tested the playground and server manually.

---

_Label `internal` added by @ntBre on 2025-03-18 21:03_

---

_Comment by @github-actions[bot] on 2025-03-18 21:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @MichaReiser by @ntBre on 2025-03-18 22:09_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-18 22:09_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/fix/mod.rs`:184 on 2025-03-19 07:48_

While the source text doesn't matter here, I'd still prefer to pass it in because the test here otherwise start panicking (at best) or produce nonsensical results) if `apply_fixes` ever starts accessing `file`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:573 on 2025-03-19 07:50_

Did this PR just open up the possibility for the parser to have fixes :)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:1248 on 2025-03-19 07:52_

Same here. I think we should pass in the actual source code to avoid panics if the code ever starts accessing `file`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:1252 on 2025-03-19 07:54_

Can we use `diagnostic.start()` instead of the `0` offset. I think that's also the default that the linter code uses elsewhere (we could change `from_diagnostic` to take an `Option<TextSize>` for the noqa offset)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/test.rs`:322 on 2025-03-19 07:58_

Do we still need the noqa offset mapping? Shouldn't `diagnostic.file` and `diagnostic.noqa_offset` already have the right `file` and offset?

---

_@MichaReiser approved on 2025-03-19 08:00_

This is a great improvement. I also think that it will make Andrew's diagnostic refactor easier because `check_path`. Thanks for working on this.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/lint.rs`:182 on 2025-03-19 11:25_

nit: I think now I'd prefer to expand the condition for `show_syntax_errors`:
```rs
if show_syntax_errors {
    Some(...)
} else {
    None
}
```
I mainly avoided that previously because it was part of a `chain` which I think is now removed.

---

_@dhruvmanila reviewed on 2025-03-19 11:25_

---

_@dhruvmanila approved on 2025-03-19 11:28_

This looks great! Thanks for doing this.

---

_@ntBre reviewed on 2025-03-19 12:22_

---

_Review comment by @ntBre on `crates/ruff_linter/src/test.rs`:322 on 2025-03-19 12:22_

Ah I think you're right. Tests were failing because some of them truncate the input path to just the file name, but I think the cause ended up being somewhere else. We can actually just print all of the messages here, and the tests still pass, but I'll leave the filter on `DiagnosticMessage`s for now.

---

_@ntBre reviewed on 2025-03-19 12:32_

---

_Review comment by @ntBre on `crates/ruff_linter/src/noqa.rs`:1248 on 2025-03-19 12:32_

Good catch, I didn't realize all of this information was already available here.

---

_@ntBre reviewed on 2025-03-19 12:39_

---

_Review comment by @ntBre on `crates/ruff_linter/src/fix/mod.rs`:184 on 2025-03-19 12:39_

Are you suggesting passing in a placeholder like `"<filename>"` but just requiring the caller to make that choice? It looks like we don't actually have a filename where this function is used, unlike `message_from_diagnostic`. We do have a `Locator` we could extract the source code argument from, though.

---

_@ntBre reviewed on 2025-03-19 12:49_

---

_Review comment by @ntBre on `crates/ruff_linter/src/fix/mod.rs`:184 on 2025-03-19 12:49_

I made this function take both a `filename: &str` and a `source: &str` to pass to the `SourceFileBuilder` and called the function throughout the tests module with `"<filename>", locator.contents(), ...`. Happy to revisit it if that's not what you had in mind, though.

---

_Merged by @ntBre on 2025-03-19 14:08_

---

_Closed by @ntBre on 2025-03-19 14:08_

---

_Branch deleted on 2025-03-19 14:08_

---
