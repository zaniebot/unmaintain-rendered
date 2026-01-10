```yaml
number: 9915
title: "Docstring formatting: Preserve tab indentation when using `indent-style=tabs`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
assignees: []
merged: true
base: main
head: preserve-docstring-tabs
created_at: 2024-02-09T20:23:49Z
updated_at: 2024-02-12T15:09:14Z
url: https://github.com/astral-sh/ruff/pull/9915
synced_at: 2026-01-10T22:57:09Z
```

# Docstring formatting: Preserve tab indentation when using `indent-style=tabs`

---

_Pull request opened by @MichaReiser on 2024-02-09 20:23_

## Summary

This PR ensures that the formatter preserves `tab`s inside of docstring when using `indent-style=tab`. 

Today, the formatter converts all docstring-indent to spaces, regardless of the configured indent-style.
This is unexpected for users and creates with `E101` (`mixed-spaces-and-tabs`). For example:

```python
def tab_argument(arg1: str) -> None:
	"""
	Arguments:
		arg1: super duper arg with 2 tabs in front
	"""
```

Gets reformatted to 

```python
def tab_argument(arg1: str) -> None:
	"""
	Arguments:
	        arg1: super duper arg with 2 tabs in front
	"""
```

Even though the user expliclty wants tab indentations. 

The formatter won't automatically convert multiples of `indent-width` spaces to tabs because it can break 
ASCII arts (and other formatting that use spaces for alignmen). 

The downside of this change is tha the formatter won't enforce a consistent indentation inside of docstrings when using `indent-style=tab`.

Fixes https://github.com/astral-sh/ruff/issues/8430


This PR does not yet address that our formatter always expands `tab` to 8 spaces (instead of the configured `indent-width`)

It's not necessary to gate this change behind preview because it doesn't change already formatted code.

## Test Plan

added tests, `cargo test`


---

_Label `formatter` added by @MichaReiser on 2024-02-09 20:24_

---

_Comment by @github-actions[bot] on 2024-02-09 20:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @BurntSushi by @MichaReiser on 2024-02-10 06:32_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-10 06:32_

---

_Marked ready for review by @MichaReiser on 2024-02-10 06:40_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/string/docstring.rs`:1596 on 2024-02-12 14:15_

What does the value signify here? Is it the number of bytes or the visual width?

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/string/docstring.rs`:1675 on 2024-02-12 14:18_

If this is returning the visual width, maybe a name other than `len` would be appropriate here? I usually expect a value returned by a method name `len()` to be usable as part of a range/offset calculation. `visual_len`? `visual_width`? `number_of_spaces`? `columns`?

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/string/docstring.rs`:1667 on 2024-02-12 14:18_

What does this method return? As in, what does the value signify?

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/string/docstring.rs`:1675 on 2024-02-12 14:19_

Can we write a comment explaining why this `unwrap()` can never fail?

If it possibly can, would `TextSize::try_from(len).ok()` make sense instead?

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/string/docstring.rs`:1579 on 2024-02-12 14:20_

I like this type. Great thinking.

---

_@BurntSushi approved on 2024-02-12 14:22_

This looks great to me! I think it might help to polish up the `Indent` type a touch, but I love the approach. :)

---

_@MichaReiser reviewed on 2024-02-12 14:39_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/docstring.rs`:1675 on 2024-02-12 14:39_

I like it. I think I'll go with `width`. It aligns nicely with `indent_width.` It then reads as `indentation.width()` (I'll rename the `Indent` type to `Indentation`

---

_@BurntSushi reviewed on 2024-02-12 14:48_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/string/docstring.rs`:1675 on 2024-02-12 14:48_

Oh yeah that's great!

---

_Merged by @MichaReiser on 2024-02-12 15:09_

---

_Closed by @MichaReiser on 2024-02-12 15:09_

---

_Branch deleted on 2024-02-12 15:09_

---
