```yaml
number: 19809
title: Detect / Prohibit Implicit String Concatenation within a list, set, tuple
type: issue
state: closed
author: lhegstrom
labels:
  - question
assignees: []
created_at: 2025-08-07T15:56:18Z
updated_at: 2025-08-07T16:10:12Z
url: https://github.com/astral-sh/ruff/issues/19809
synced_at: 2026-01-12T15:54:57Z
```

# Detect / Prohibit Implicit String Concatenation within a list, set, tuple

---

_@lhegstrom_

### Summary

It's possible to write valid python code that looks like:

```python
my_list = [
    "a" #missing comma 
    "b",
    "c",
]
```

This performs an implicit string concatenation within the list at runtime:

`my_list = ["ab", "c"]`

Same for tuples and sets.

There is a single line implicit string concatenation rule:
https://docs.astral.sh/ruff/rules/single-line-implicit-string-concatenation/

But, this does not seem to pick up on implicit string concatenation within a iterable.

And, of course, if your list/set/tuple are long enough, black reformats them into a vertical orientation.

I've run into issues multiple times where someone sets some sort of configuration as a list or tuple, misses a comma, and the values are used to query against a database, etc. The error often goes undetected for a while before someone notices. Would love to have a rule that prohibits this implicit string concatenation within an iterable


---

_Comment by @MichaReiser on 2025-08-07 15:59_

You can set [`allow-multiline`](https://docs.astral.sh/ruff/settings/#lintflake8-implicit-str-concat) to disallow multiline implicit string concatenation. However, this will disallow any multiline implicit string concatenation and not just in lists or sets. 

---

_Label `question` added by @MichaReiser on 2025-08-07 15:59_

---

_Comment by @lhegstrom on 2025-08-07 16:10_

Ah, it looks like using both:
ISC001 and ISC002 + the disallow multiline resolves this. Thank you!

Closing.

---

_Closed by @lhegstrom on 2025-08-07 16:10_

---
