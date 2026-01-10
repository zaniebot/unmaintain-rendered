```yaml
number: 20197
title: "[`flynt`] Use triple quotes for joined raw strings with newlines (`FLY002`)"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-FLY002-autofix-error-with-raw-strings
created_at: 2025-09-01T18:47:51Z
updated_at: 2025-09-18T17:18:29Z
url: https://github.com/astral-sh/ruff/pull/20197
synced_at: 2026-01-10T17:40:28Z
```

# [`flynt`] Use triple quotes for joined raw strings with newlines (`FLY002`)

---

_Pull request opened by @TaKO8Ki on 2025-09-01 18:47_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #19887

- flynt(FLY002): When joining only string constants, upgrade raw single-quoted strings to raw triple-quoted if the resulting
content contains a newline.
- Choose a safe triple-quote delimiter by switching to the opposite quote style if the preferred triple appears inside the
content.
- Update FLY002 snapshot to include the `\n'.join([r'line1','line2'])` case.

## Test Plan

I've added one test case to FLY002.py.

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-09-01 18:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs`:98 on 2025-09-11 17:35_

I think we could use a combination of [`StringFlags::quote_str`](https://github.com/astral-sh/ruff/blob/59c8fda3f8f3bf2cc5c1ae34e7ca9dbea4d0278f/crates/ruff_python_ast/src/nodes.rs#L746) and [`Quote::opposite`](https://github.com/astral-sh/ruff/blob/59c8fda3f8f3bf2cc5c1ae34e7ca9dbea4d0278f/crates/ruff_python_ast/src/str.rs#L37) to simplify this section a bit.

Should we add a test case (or a few test cases) for this behavior?

I also think we may need to check for both `'''` and `"""` in every string. We could end up with a pathological case like this:

```py
'\n'.join([r"raw string", '<""">', "<'''>"])
```

which we can't join into a single string with either delimiter. I think if we have a raw string, and the joined string contains our attempted triple quotes, we'll just have to bail out without a fix.

CPython escapes some of the quotes in this case:

```pycon
>>> '\n'.join([r"raw string", '<""">', "<'''>"])
'raw string\n<""">\n<\'\'\'>'
```

I think we could also do that with our [`UnicodeEscape`](https://github.com/astral-sh/ruff/blob/d412f49fb2ec5af376cba15fd5c3348a9d0c20e0/crates/ruff_python_literal/src/escape.rs#L50) type, but I'd be fine with, or even prefer, avoiding the fix in that case.

---

_@ntBre requested changes on 2025-09-11 17:41_

Thanks! I think we can simplify a bit, and there are also some edge cases we still need to handle.

---

_Review comment by @TaKO8Ki on `crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs`:98 on 2025-09-11 18:16_

Thank you for the suggestion. I have addressed your comment.

---

_@TaKO8Ki reviewed on 2025-09-11 18:16_

---

_Review requested from @ntBre by @TaKO8Ki on 2025-09-11 18:43_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs`:94 on 2025-09-18 16:43_

nit: I think we can combine the `contains` checks:


```suggestion
        if flags.prefix().is_raw() && content.contains(['\n', '\r']) {
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs`:105 on 2025-09-18 16:56_

I think something like this would help with https://github.com/astral-sh/ruff/issues/19837 too, but it looks sufficiently different that we should leave it for a separate PR. I think we'd need this kind of check in one of the `helpers` below.

---

_@ntBre approved on 2025-09-18 16:59_

Thank you!

I'm a bit surprised that we don't offer a diagnostic at all if constructing the fix fails, but we can leave that as a separate issue too. I couldn't tell for sure from my brief read of the code, but maybe `build_fstring` is actually necessary to validate that the diagnostic applies too, not just the fix.

---

_Label `bug` added by @ntBre on 2025-09-18 16:59_

---

_Label `fixes` added by @ntBre on 2025-09-18 16:59_

---

_Renamed from "[`flynt`] Use raw triple quotes for joined raw strings with newlines" to "[`flynt`] Use triple quotes for joined raw strings with newlines (`FLY002`)" by @ntBre on 2025-09-18 17:00_

---

_@TaKO8Ki reviewed on 2025-09-18 17:07_

---

_Review comment by @TaKO8Ki on `crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs`:105 on 2025-09-18 17:07_

Ok. I will try it in a different pull request:)

---

_Merged by @ntBre on 2025-09-18 17:18_

---

_Closed by @ntBre on 2025-09-18 17:18_

---
