---
number: 17710
title: "ERA001 & RUF003 false positives on mathematical expressions in comments"
type: issue
state: open
author: nicoonoclaste
labels:
  - rule
assignees: []
created_at: 2025-04-29T14:42:31Z
updated_at: 2025-04-30T06:44:02Z
url: https://github.com/astral-sh/ruff/issues/17710
synced_at: 2026-01-07T13:12:16-06:00
---

# ERA001 & RUF003 false positives on mathematical expressions in comments

---

_Issue opened by @nicoonoclaste on 2025-04-29 14:42_

### Summary

I noticed that ERA001 triggers on mathematical expressions (as produced by `unicodeit`, or manually authored) in comments, for example:
```py
def encode_int(n: int) -> bytes:
	"""A non-prefix-free, variable-length, optimal encoding of (unsigned) integers

	The code puts 256ⁱ integers in the length-i byte strings (in order),
	 then the next 256ⁱ⁺¹ integers in length-(i+1) strings, etc.
	"""
	if n < 0:
		raise ValueError(f"encode_int expected a non-negative integer, got {n}")

	# minimum i s.t.      n < ∑ 256ʲ = (256ⁱ⁺¹ − 1) / 255
	#  <=>         255n + 1 < 256ⁱ⁺¹
	#  <=> log₂₅₆(255n + 1) < i + 1
	i = byte_length(255 * n + 1) - 1  # no greater than byte_length(n)
	return (n - (256**i - 1) // 255).to_bytes(byteorder="big", length=i)
```

Moreover, the use of `−` (mathematical “minus sign” in Unicode) triggers RUF003.

This is a recurring issue, as I often need to document the mathematics behind the algorithms my code implements.

Would it be possible to
- make RUF003 ignore the [Mathematical Operators] (U+2200–U+22FF) and [Supplemental Mathematical Operators] (U+2A00–U+2AFF) blocks, at least within comments, and
- improve ERA001's heuristic, not to cause false positives on mathematical expression which are not syntactically-valid Python.

[Mathematical Operators]: https://en.wikipedia.org/wiki/Mathematical_Operators_(Unicode_block)
[Supplemental Mathematical Operators]: https://en.wikipedia.org/wiki/Supplemental_Mathematical_Operators

### Version

ruff 0.11.7

---

_Comment by @nicoonoclaste on 2025-04-29 14:42_

Oops, accidentally hit Ctrl+Return before I was done  😅 

---

_Comment by @nicoonoclaste on 2025-04-29 14:54_

Wikipedia has a [more-extensive list](https://en.wikipedia.org/wiki/Mathematical_operators_and_symbols_in_Unicode) of commonly-used mathematical symbols

---

_Comment by @ntBre on 2025-04-29 20:37_

For ERA001, it looks like the `=` and `()` are causing the false positive here. These are included in the `POSITIVE_CASES` regex set, so I think we'd need a new heuristic to avoid lines like this.

https://github.com/astral-sh/ruff/blob/888c45584d483146dc38438e5ec7975f6b6ec502/crates/ruff_linter/src/rules/eradicate/detection.rs#L60-L72

It looks like RUF003 already has a config option to allow specific operators, and the minus sign is even used as an example: https://docs.astral.sh/ruff/settings/#lint_allowed-confusables

It might be a bit tedious to include all of the mathematical symbols, but at least it's possible. Otherwise, this seems to be the intention behind the rule, so it's more of a true positive, in my opinion.



---

_Label `rule` added by @ntBre on 2025-04-29 20:37_

---

_Comment by @MichaReiser on 2025-04-30 06:44_

The "proper" solution is probably to rework ERA001 and make it stricter (e.g. analyze consecutive commented lines together instead of line by line which causes the false positive here). 

I'm not sure if there's a good short-term fix.

We're aware that RUF003 and similar are overly strict right now, see https://github.com/astral-sh/ruff/issues/14433

---
