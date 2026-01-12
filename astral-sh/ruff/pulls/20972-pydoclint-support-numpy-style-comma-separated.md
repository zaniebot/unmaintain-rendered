```yaml
number: 20972
title: "[`pydoclint`] Support NumPy-style comma-separated parameters (`DOC102`)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-20959
created_at: 2025-10-19T21:51:19Z
updated_at: 2025-11-12T07:44:41Z
url: https://github.com/astral-sh/ruff/pull/20972
synced_at: 2026-01-12T15:57:13Z
```

# [`pydoclint`] Support NumPy-style comma-separated parameters (`DOC102`)

---

_@danparizher_

## Summary

Fixes #20959. Enhanced the DOC102 rule to recognize and validate multiple parameters listed together in NumPy-style docstrings using comma separation.

## Problem Analysis

The existing DOC102 rule failed to detect extraneous parameters when NumPy-style docstrings combined multiple parameters with commas (e.g., `x1, x2 : object`), causing false negatives.


## Approach

Updated the `parse_parameters_numpy` function to split comma-separated parameter names and validate each individually, added comprehensive test cases covering the original issue and edge cases, and ensured code compliance with project standards.

---

_Comment by @github-actions[bot] on 2025-10-19 22:03_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:672 on 2025-10-22 16:22_

I think this is just
```suggestion
                                + param_part_trimmed.text_len();
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:668 on 2025-10-22 16:27_

```suggestion
                                + TextSize::try_from(param_start_in_line).unwrap();
```

This is one of the rare cases where plain `unwrap` is fine. Any usize within a document that we're linting (and in this case an offset within a single line) should definitely be convertible into a u32 or `TextSize`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:665 on 2025-10-22 16:47_

Let's just make this an `unwrap` too. We're searching for a substring in a line we just split, so it should be okay. And if we end up unwrapping to zero  somehow, the rendered range could look really bad. It might be worth a short safety comment to that effect, e.g.:

> Safety: searching for a substring we just split above

Ideally I feel like we'd just update the `current_pos` or a similar offset value as we go instead of `find`ing after the fact, but I think that's hard to do with `split`, so this seems okay.

---

_@ntBre reviewed on 2025-10-22 16:48_

Thanks! Just a few nits

---

_Label `rule` added by @ntBre on 2025-10-22 16:48_

---

_Label `preview` added by @ntBre on 2025-10-22 16:48_

---

_Review requested from @ntBre by @danparizher on 2025-10-24 21:35_

---

_Comment by @augustelalande on 2025-10-30 22:37_

Can you add a test case for a type less docstring with comma, e.g.:
```python
def add_numbers(a, b):
    """
    Adds two numbers and returns the result.

    Parameters
    ----------
    a, b
        The numbers to add.

    Returns
    -------
    int
        The sum of the two numbers.
    """
    return a + b
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:664 on 2025-11-10 12:37_

I suspect that the `trim_end` call here now is unnecessary, given that you also trim each parameter part.

---

_@MichaReiser reviewed on 2025-11-10 12:37_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:673 on 2025-11-10 12:40_

I'd be inclined to keep track of the offset instead of finding the parameter aggain. 

```
let mut current_offset = TextSize::MIN;

for param_part in ... {
	...

	let param_start_in_line = current_offset + (param_part.text_len() - param_part_trimmed.text_len());

	current_offset = current_offset + param_part.text_len() + ','.text_len()
}
```

---

_@MichaReiser reviewed on 2025-11-10 12:40_

---

_@MichaReiser reviewed on 2025-11-10 12:41_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:684 on 2025-11-10 12:41_

```suggestion
                                range: TextRange::at(
                                    content_start + param_start,
                                    param_part_trimmed.text_len(),
                                ),
```

---

_@MichaReiser approved on 2025-11-10 12:48_

---

_@danparizher reviewed on 2025-11-10 20:54_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:664 on 2025-11-10 20:54_

Removing it causes incorrect column positions in the diagnostic ranges, from my testing.

Even though we `trim()` each parameter part, the offset calculation happens before trimming, so the trailing whitespace in `before_colon` affects the positions.

---

_@MichaReiser reviewed on 2025-11-11 11:07_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:677 on 2025-11-11 11:07_

This seems unnecessarily complicated to go from `TextSize -> u32 -> TextSize`. Can you say more why you added that?

---

_@danparizher reviewed on 2025-11-11 21:26_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:677 on 2025-11-11 21:26_

Oversight on my part - Since both `param_part.text_len()` and `param_part_trimmed.text_len()` return `TextSize`, we can subtract them directly without the conversion.

---

_@MichaReiser approved on 2025-11-12 07:29_

---

_Merged by @MichaReiser on 2025-11-12 07:29_

---

_Closed by @MichaReiser on 2025-11-12 07:29_

---

_Branch deleted on 2025-11-12 07:44_

---
