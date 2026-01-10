```yaml
number: 18616
title: "[`flake8-comprehensions`] Fix `C420` to prepend whitespace when needed"
type: pull_request
state: merged
author: robsdedude
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix/18599-C420-prepend-padding
created_at: 2025-06-10T19:55:33Z
updated_at: 2025-06-30T19:00:35Z
url: https://github.com/astral-sh/ruff/pull/18616
synced_at: 2026-01-10T18:39:08Z
```

# [`flake8-comprehensions`] Fix `C420` to prepend whitespace when needed

---

_Pull request opened by @robsdedude on 2025-06-10 19:55_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
This PR fixes rule C420's fix. The fix replaces `{...}` with `dict....(...)`. Therefore, if there is any identifier or such right before the fix, the fix will fuse that previous token with `dict...`.

The example in the issue is
```python
0 or{x: None for x in "x"}
# gets "fixed" to
0 ordict.fromkeys(iterable)
```

## Related Issues

Fixes: https://github.com/astral-sh/ruff/issues/18599


---

_Comment by @github-actions[bot] on 2025-06-10 20:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@robsdedude reviewed on 2025-06-10 20:20_

---

_Review comment by @robsdedude on `crates/ruff_python_parser/src/lexer.rs`:1637 on 2025-06-10 20:20_

Not sure you're fine with this PR increasing the coupling of the crates. However, I didn't want to duplicate that logic. But maybe there's even a better general approach for fixing the issue that works on a higher logical level than chars.

---

_Marked ready for review by @robsdedude on 2025-06-10 20:20_

---

_Review requested from @MichaReiser by @robsdedude on 2025-06-10 20:21_

---

_Review requested from @dhruvmanila by @robsdedude on 2025-06-10 20:21_

---

_Label `bug` added by @ntBre on 2025-06-10 20:22_

---

_Label `fixes` added by @ntBre on 2025-06-10 20:22_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/lexer.rs`:1637 on 2025-06-18 18:40_

This is a neat find, I didn't know about this function before.

I think I would have instead reached for something more like was done in https://github.com/astral-sh/ruff/pull/17648 (checking if two token ranges are adjacent). Would that work here too?

I'm not totally against using the lexer function if not, but I'm not sure we really want to make it `pub` like you  said.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_dict_comprehension_for_iterable.rs`:127 on 2025-06-18 18:46_

It would be nice to avoid the `format!` call if we don't need any padding, so I'd probably just in-line `fix_padding` and put this `format!` in the `true` branch (or at least change the signature of `fix_padding`). I'm picturing something like this:

```rust
let replacement = checker.generator() /* ...the rest of that code */;
let replacement = if needs_padding {
    format!(" {replacement}")
} else {
    replacement
};
```

---

_@ntBre reviewed on 2025-06-18 18:49_

Thanks! This looks good overall, just a couple of suggestions.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1637 on 2025-06-23 09:04_

Could we use `edits::pad_start` instead or `is_identifier`. I'd prefer to not make this function public

---

_@MichaReiser reviewed on 2025-06-23 09:04_

---

_Review comment by @robsdedude on `crates/ruff_python_parser/src/lexer.rs`:1637 on 2025-06-27 22:17_

`pad_start` sounds exactly like what we want... However, looking at its implementation I suspect (not tested yet) that it's implemented incorrectly. It only considers `is_ascii_alphabetic`. Python's identifiers allow much more than ASCII alphabetic characters, which is exactly why I reached for `is_identifier_continuation`. How do you suggest to proceed? My gut feeling is
 * use `pad_start` for this PR
 * open a separate issue to discuss fixing `pad_start`
 
 What `is_identifier` are you referring to? There are multiple functions with that name in the repo :innocent: If you mean `ruff_python_stdlib::identifiers::is_identifier`, I suppose we could, but it'd require looking at more than just the preceding character which might only be a valid identifier continuation, but not a start (e.g., a digit). Sounds more costly and error prone.

---

_@robsdedude reviewed on 2025-06-27 22:17_

---

_@robsdedude reviewed on 2025-06-27 22:21_

---

_Review comment by @robsdedude on `crates/ruff_python_parser/src/lexer.rs`:1637 on 2025-06-27 22:21_

Well, maybe in this case it's correct because we only care about preceding keywords, which only are contain ASCII alphabetic chars.

---

_@robsdedude reviewed on 2025-06-27 22:49_

---

_Review comment by @robsdedude on `crates/ruff_python_parser/src/lexer.rs`:1637 on 2025-06-27 22:49_

Yeah, after looking at some examples in the code-base as well as Python's [grammar for expressions](https://docs.python.org/3/reference/expressions.html#grammar-token-python-grammar-comparison), I'm fairly convinced, that `pad`, `pad_start`, and `pad_end` are fine as long as all edits that are padded are replacing a full expression as those cannot be surrounded by arbitrary identifiers as far as I can tell.

---

_Review requested from @ntBre by @robsdedude on 2025-06-27 23:11_

---

_@ntBre approved on 2025-06-30 16:38_

Thanks! This looks really nice with `pad_start`. Thanks for double checking that it was working correctly too.

---

_Renamed from "[`flake8_comprehensions`] Fix `C420` to prepend whitespace when needed" to "[`flake8-comprehensions`] Fix `C420` to prepend whitespace when needed" by @ntBre on 2025-06-30 16:38_

---

_Merged by @ntBre on 2025-06-30 16:38_

---

_Closed by @ntBre on 2025-06-30 16:38_

---

_Branch deleted on 2025-06-30 19:00_

---
