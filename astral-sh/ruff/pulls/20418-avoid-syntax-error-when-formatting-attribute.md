```yaml
number: 20418
title: Avoid syntax error when formatting attribute expressions with outer parentheses, parenthesized value, and trailing comment on value
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: parens-comment-attr-fmt-bug
created_at: 2025-09-15T14:53:33Z
updated_at: 2025-11-17T15:11:36Z
url: https://github.com/astral-sh/ruff/pull/20418
synced_at: 2026-01-12T15:57:01Z
```

# Avoid syntax error when formatting attribute expressions with outer parentheses, parenthesized value, and trailing comment on value

---

_@dylwil3_

Closes #19350 

I'm not sure if this is the expected/desired output. Without parentheses around the value, we have:

```python
# unformatted
variable = (
    something # a comment
    .first_method("some string")
)

# formatted
variable = something.first_method("some string")  # a comment
```

It's a little unclear to me... it almost feels like in _both_ cases (when `something` does or does not have parentheses) we should maintain the break, since that's what I would expect from a cursory reading of https://docs.astral.sh/ruff/formatter/black/#trailing-end-of-line-comments

---

_Label `bug` added by @dylwil3 on 2025-09-15 14:53_

---

_Label `formatter` added by @dylwil3 on 2025-09-15 14:53_

---

_Comment by @github-actions[bot] on 2025-09-15 15:08_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Marked ready for review by @dylwil3 on 2025-09-15 15:16_

---

_Review requested from @MichaReiser by @dylwil3 on 2025-09-15 15:16_

---

_Comment by @MichaReiser on 2025-09-16 14:42_

Thank you. 

The comment placement is a bit weird if `something` isn't parenthesized, but the comment is very long:

```python
variable = (
    (something) # a commentdddddddddddddddddddddddddddddd
    .first_method("some string")
)


variable = (
    something # a commentdddddddddddddddddddddddddddddd
    .first_method("some string")
)
```

Formatted

```
variable = (
    (something)  # a commentdddddddddddddddddddddddddddddd
    .first_method("some string")
)


variable = something.first_method(  # a commentdddddddddddddddddddddddddddddd
    "some string"
)
```

We might want to spent a little time to see if we can preserve the comment placement closer to where it used to be (by e.g. inserting a hard line break after the attribute name or using a line suffix boundary). But this might be harder than it looks and is something we could fix separately.

The fix otherwise looks good, e.g. it also fixes this code that previously produced invalid syntax (may be worth adding as a test to showcase that the issue isn't specific to assignment statements):

```python
if (
    (something)  # a commentdddddddddddddddddddddddddddddd
    .first_method("some string")
): pass
```


---

_@MichaReiser reviewed on 2025-09-16 14:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_attribute.rs`:182 on 2025-09-16 14:45_

We'll now also use the new layout if `value` is parenthesized and has a trailing comment like this

```python
variable = (
    (something # a comment  
    ).fist_method("some string")
)
```

I think that's fine but it might be worth adding a few more tests.

For example, this snippet now gets formatted differently:


```python
if (
    (something # a comment  
    ).fist_method("some string") # second comment
): pass
```


Overall, maybe add a few more permutations with multiple comments (or at least play with it in the playground)

---

_Comment by @dylwil3 on 2025-11-14 22:24_

I've added some more tests, and I'm personally okay with the edge cases that produce slightly awkward formatting (in the name of getting rid of this syntax error). But let me know if you want me to try to tweak anything as part of this PR.

---

_Review requested from @MichaReiser by @dylwil3 on 2025-11-14 22:24_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_attribute.rs`:182 on 2025-11-15 18:11_

Do we need to limit this to end of line comments only? Can you add a test where a value has both (unless that's not possible)

---

_@MichaReiser approved on 2025-11-15 18:11_

---

_@dylwil3 reviewed on 2025-11-17 14:53_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/expression/expr_attribute.rs`:182 on 2025-11-17 14:53_

It's possible to have both (and we get a syntax error on main after formatting, but not in this PR). But you're right that we only need to use `Multiline` here if there's a trailing end of line comment, so I've narrowed the condition.

Added a test with both and a test just with trailing own-line comment (the latter formats with the same IR as on `main`, and without a syntax error, now that we narrowed the condition).

---

_Merged by @dylwil3 on 2025-11-17 15:11_

---

_Closed by @dylwil3 on 2025-11-17 15:11_

---
