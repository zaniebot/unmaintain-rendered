```yaml
number: 19774
title: "[`pyupgrade`] Fix Unicode escape sequence handling in format string parsing"
type: pull_request
state: closed
author: danparizher
labels:
  - bug
  - fixes
assignees: []
base: main
head: fix-19771
created_at: 2025-08-05T22:16:38Z
updated_at: 2025-09-30T07:13:29Z
url: https://github.com/astral-sh/ruff/pull/19774
synced_at: 2026-01-12T15:56:46Z
```

# [`pyupgrade`] Fix Unicode escape sequence handling in format string parsing

---

_@danparizher_

## Summary

Fixes #19771


---

_Comment by @github-actions[bot] on 2025-08-05 22:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-08-06 00:57_

---

_Label `fixes` added by @ntBre on 2025-08-06 00:57_

---

_Review requested from @ntBre by @ntBre on 2025-08-06 00:58_

---

_Review comment by @ntBre on `crates/ruff_python_literal/src/format.rs`:603 on 2025-08-06 16:38_

I don't think we need to collect here:


```suggestion
                for (i, c) in cur_text.chars().iter().enumerate().skip(3) {
```

---

_Review comment by @ntBre on `crates/ruff_python_literal/src/format.rs`:606 on 2025-08-06 16:40_

I don't think we need to count the braces, but we need to return some kind of error if we encounter another open brace, based on a quick test in Python. Maybe there's a way to make this work, but it seems to me that braces aren't allowed inside of a `\N{...}` escape:

```pycon
>>> "\N{{angle}}".format(angle="angle")
  File "<python-input-0>", line 1
    "\N{{angle}}".format(angle="angle")
    ^^^^^^^^^^^^^
SyntaxError: (unicode error) 'unicodeescape' codec can't decode bytes in position 0-9: unknown Unicode character name
```

---

_Review comment by @ntBre on `crates/ruff_python_literal/src/format.rs`:618 on 2025-08-06 16:41_

If we `push_str("\\N")` at the start, we can just push individual characters as we go instead of bumping `end_pos`.

---

_Review comment by @ntBre on `crates/ruff_python_literal/src/format.rs`:1093 on 2025-08-06 16:43_

Ah yeah, this is an error in CPython, so this is the case I think we should return an `Err` in.

```pycon
>>> "\N{LATIN {SMALL} LETTER A}"
  File "<python-input-2>", line 1
    "\N{LATIN {SMALL} LETTER A}"
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
SyntaxError: (unicode error) 'unicodeescape' codec can't decode bytes in position 0-15: unknown Unicode character name
```

---

_@ntBre reviewed on 2025-08-06 16:46_

Thanks for looking into this!

I'm a bit wary of changing this parsing code, but I had a few nits if we do go this route. We'd probably also want @MichaReiser to take a look here.

This code isn't _super_ widely used, only occurring in ~3 rules or so, so it's not as foundational as I thought initially, but we should still be careful.

---

_Review requested from @ntBre by @danparizher on 2025-08-07 19:52_

---

_Comment by @MichaReiser on 2025-08-08 10:06_

I don't think I'll have time anytime soon to review this in detail and I'm also not familiar with this code or the logic around it. @ntBre you're right to be wary. There have been plenty of bugs around f-string handling, https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20is%3Aissue%20state%3Aclosed%20UP032

---

_Review comment by @MichaReiser on `crates/ruff_python_literal/src/format.rs`:601 on 2025-08-08 10:07_

These seems guaranteed by the `starts_with` call on line 597`

---

_@MichaReiser reviewed on 2025-08-08 10:07_

---

_Review comment by @MichaReiser on `crates/ruff_python_literal/src/format.rs`:606 on 2025-08-08 10:08_

It feels a bit silly to update `result_string` only to undo the changes right after. Can we avoid that?

---

_@MichaReiser reviewed on 2025-08-08 10:08_

---

_Review comment by @MichaReiser on `crates/ruff_python_literal/src/format.rs`:625 on 2025-08-08 10:09_

Isn't `chars.as_str` always guaranteed to be empty if `chars.next()` is `None`?

---

_@MichaReiser reviewed on 2025-08-08 10:09_

---

_Comment by @IDrokin117 on 2025-08-10 13:53_

@danparizher 
I've been looking into the same problem while working on the issue https://github.com/astral-sh/ruff/issues/19792
Please examine the potential problems this PR could cause
1. Consider the example below. ruff 0.12.8 doesn't fix it due to missing format keyword `snowman` ([playground](https://play.ruff.rs/?secondary=AST)). However, with the proposed changes, the format will be replaced by f-string. I suppose it is wrong behavior.
``` python
# ruff 0.12.8 
print("\\N{snowman} {}".format(1)) # doesn't apply UP032
```
```python
# With the fix, UP032 will be applied
print("\\N{snowman} {}".format(1)) # -> print(f"\\N{snowman} {1}")
```
2. While the fix will resolve the first (1) case, it still has a problem with the second (2) case.
```python
# ruff 0.12.8 
print("\N{snowman} {snowman}".format(snowman=3)) # -> print(f"\N{snowman} {3}") # (1)
```
```python
# With the fix, here first {snowman} must also be substituted with {1}.
print("\\N{snowman} {snowman}".format(snowman=1)) # -> print(f"\\N{snowman} {1}") # (2)
```
[
Here](https://github.com/IDrokin117/ruff/commit/4b126db73a634f6573906207bdc8e445d5116525) you can find my solution that correctly addresses the cases mentioned above. Feel free to reuse it in your PR if it makes sense.

---

_Comment by @MichaReiser on 2025-09-30 07:13_

Closing due to inactivity. I can re-open this PR if you plan to work on this. If anyone else is interested to work in this, feel free to submit a new PR that takes the above feedback into account.

---

_Closed by @MichaReiser on 2025-09-30 07:13_

---
