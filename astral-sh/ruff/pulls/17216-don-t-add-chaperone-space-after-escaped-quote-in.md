```yaml
number: 17216
title: "Don't add chaperone space after escaped quote in triple quote"
type: pull_request
state: merged
author: maxmynter
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: unex-space
created_at: 2025-04-05T05:30:04Z
updated_at: 2025-04-11T08:22:32Z
url: https://github.com/astral-sh/ruff/pull/17216
synced_at: 2026-01-12T15:56:00Z
```

# Don't add chaperone space after escaped quote in triple quote

---

_@maxmynter_

Closes #16640

## Summary
Added a check for escaped quotes in triple quotes to determine if chaperone spaces are needed.
This prevents unwanted formatting of valid docstrings that end in escaped quotes.

## Test Plan
Added a unit test testing that docstrings ending in escaped double quotes are not reformatted.

---

_Review requested from @MichaReiser by @maxmynter on 2025-04-05 05:30_

---

_Comment by @github-actions[bot] on 2025-04-05 05:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `breaking` added by @MichaReiser on 2025-04-05 06:36_

---

_Label `formatter` added by @AlexWaygood on 2025-04-05 22:18_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/docstring.rs`:1604 on 2025-04-07 09:23_

I don't think adding the check to this branch makes sense because 

```
trim_end.chars().rev().take_while(|c| *c == '\\').count() % 2 == 1
```
will always return `false` if the string ends with `\'` or `\"` 

Or am I misinterpreting this code?

I think it also incorrectly returns `false` for 

```py
"""Hello "World\\""""
```

Nit: It would also be nice to avoid the allocating `format` call here.





---

_@MichaReiser reviewed on 2025-04-07 09:23_

---

_@maxmynter reviewed on 2025-04-08 13:31_

---

_Review comment by @maxmynter on `crates/ruff_python_formatter/src/string/docstring.rs`:1604 on 2025-04-08 13:31_

Thanks for flagging this; you're right. It seems i underestimated edge cases and then got caught up in premature refactoring without sufficient test cases.

I've added a bunch more tests in [d6e332b](https://github.com/astral-sh/ruff/pull/17216/commits/d6e332bf3a2fb6790f20e552e8db6feb89621ba8) and fixed the implementation in [5a5f55c](https://github.com/astral-sh/ruff/pull/17216/commits/5a5f55ce0cb7d383f35699243445f09a0ee12611).

---

_Review requested from @MichaReiser by @maxmynter on 2025-04-08 13:32_

---

_@MichaReiser reviewed on 2025-04-09 07:12_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/docstring.rs`:1619 on 2025-04-09 07:12_

I find the way the `if`s are nested hard to reason about because checking `count_consecutive_chars_from_end(trim_end, '\\') % 2` is always false if `trim_end.ends_with(flags.quote_style().as_char())`

Can't we simplify this to:

```
if count_consecutive_chars_from_end(trim_end, '\\') % 2 == 1 {
    // Odd backslash count; chaperone avoids escaping closing quotes
    return true;
}

if let Some(before_quote) = trim_end.rstrip_prefix(flags.quote_style().as_char()) {
	if count_consecutive_chars_from_end(before_quote, '\\') % 2 == 1 {
    // Even backslash count preceding quote; chaperone avoids dangling
    return true;
	}
}

false
```



---

_@maxmynter reviewed on 2025-04-09 11:26_

---

_Review comment by @maxmynter on `crates/ruff_python_formatter/src/string/docstring.rs`:1619 on 2025-04-09 11:26_

With slight adaptations, yes, we can simplify:
- used `strip_suffix` instead of `rstrip_prefix` (which doesn't exist).
- Check for even not odd backslash count before the closing quote.

Implemented in [ad8604f](https://github.com/astral-sh/ruff/pull/17216/commits/ad8604f6b09ef25916ef683881f0a51e20f1c097)

---

_Label `breaking` removed by @MichaReiser on 2025-04-11 07:51_

---

_Label `preview` added by @MichaReiser on 2025-04-11 07:51_

---

_@MichaReiser reviewed on 2025-04-11 07:59_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_chaperones.py`:5 on 2025-04-11 07:59_

This file doesn't test the chaperone handling except for the first string because all subsequent strings aren't docstrings.

---

_Merged by @MichaReiser on 2025-04-11 08:21_

---

_Closed by @MichaReiser on 2025-04-11 08:21_

---
