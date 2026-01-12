```yaml
number: 19647
title: "[`ruff`] Preserve relative whitespace in multi-line expressions (`RUF033`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-19581
created_at: 2025-07-30T19:50:52Z
updated_at: 2025-08-27T19:32:08Z
url: https://github.com/astral-sh/ruff/pull/19647
synced_at: 2026-01-12T15:56:44Z
```

# [`ruff`] Preserve relative whitespace in multi-line expressions (`RUF033`)

---

_@danparizher_

## Summary

Fixes #19581

I decided to add in a `indent_first_line` function into [`textwrap.rs`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_trivia/src/textwrap.rs), as it solely focuses on text manipulation utilities. It follows the same design as `indent()`, and there may be situations in the future where it can be reused as well.

---

_Label `bug` added by @ntBre on 2025-07-30 19:58_

---

_Label `fixes` added by @ntBre on 2025-07-30 19:58_

---

_Review requested from @ntBre by @ntBre on 2025-07-30 19:58_

---

_Comment by @github-actions[bot] on 2025-07-30 20:02_

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

_Review comment by @ntBre on `crates/ruff_python_trivia/src/textwrap.rs`:96 on 2025-08-11 19:27_

Unless I'm missing something, this example looks the same as the case above. I think this would only apply to input starting with whitespace?


```suggestion
/// When indenting, trailing whitespace is stripped from the prefix.
/// This means that empty lines remain empty afterwards:
///
/// ```
/// # use ruff_python_trivia::textwrap::indent_first_line;
///
/// assert_eq!(indent_first_line("\n\n\nSecond line.\n", "  "),
///            "\n\n\nSecond line.\n");
/// ```
```

Like that maybe? It looks like you have a test for that below.

---

_Review comment by @ntBre on `crates/ruff_python_trivia/src/textwrap.rs`:114 on 2025-08-11 19:31_

nit: I think this might be nice as a let-else:


```suggestion
    let Some(first_line) = lines.next() else {
        return Cow::Borrowed(text);
```

---

_@ntBre approved on 2025-08-11 19:38_

Thank you, this looks good to me! I just had a couple of nits. It might also be nice to throw in the triple-quoted string case as a RUF033 test too, just to double check.

---

_Comment by @ntBre on 2025-08-12 14:56_

Ah the test failures are just a merge conflict with our new diagnostic format. Would you mind rebasing/merging `main` and then accepting the new snapshots? The new results look correct to me in the CI output.

---

_Review requested from @ntBre by @danparizher on 2025-08-15 18:46_

---

_Comment by @ntBre on 2025-08-27 19:12_

Looks great, thanks! Sorry I forgot to come back to this after the rebase. I just pushed one additional test case like the reported issue and confirmed that everything looks good.

---

_Renamed from "[`RUF`] Fix indentation to preserve relative whitespace in multi-line expressions (`RUF033`)" to "[`ruff`] Preserve relative whitespace in multi-line expressions (`RUF033`)" by @ntBre on 2025-08-27 19:13_

---

_Merged by @ntBre on 2025-08-27 19:15_

---

_Closed by @ntBre on 2025-08-27 19:15_

---

_Branch deleted on 2025-08-27 19:32_

---
