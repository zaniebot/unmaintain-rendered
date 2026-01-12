```yaml
number: 20867
title: Fix syntax error false positives for escapes and quotes in f-strings
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: brent/pep-701-f-string-fix
created_at: 2025-10-14T16:33:54Z
updated_at: 2025-10-15T13:23:18Z
url: https://github.com/astral-sh/ruff/pull/20867
synced_at: 2026-01-12T15:57:11Z
```

# Fix syntax error false positives for escapes and quotes in f-strings

---

_@ntBre_

Summary
--

Fixes #20844 by refining the unsupported syntax error check for [PEP 701]
f-strings before Python 3.12 to allow backslash escapes and escaped outer quotes
in the format spec part of f-strings. These are only disallowed within the
f-string expression part on earlier versions. Using the examples from the PR:

```pycon
>>> f"{1:\x64}"
'1'
>>> f"{1:\"d\"}"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: Invalid format specifier '"d"' for object of type 'int'
```

Note that the second case is a runtime error, but this is actually avoidable if
you override `__format__`, so despite being pretty weird, this could actually be
a valid use case.

```pycon
>>> class C:
...     def __format__(*args, **kwargs): return "<C>"
...
>>> f"{C():\"d\"}"
'<C>'
```

At first I thought narrowing the range we check to exclude the format spec would
only work for escapes, but it turns out that cases like `f"{1:""}"` are already
covered by an existing `ParseError`, so we can just narrow the range of both our
escape and quote checks.

Our comment check also seems to be working correctly because it's based on the
actual tokens. A case like [this](https://play.ruff.rs/9f1c2ff2-cd8e-4ad7-9f40-56c0a524209f):

```python
f"""{1:# }"""
```

doesn't include a comment token, instead the `#` is part of an
`InterpolatedStringLiteralElement`.

Test Plan
--

New inline parser tests

[PEP 701]: https://peps.python.org/pep-0701/


---

_Label `bug` added by @ntBre on 2025-10-14 16:33_

---

_Label `parser` added by @ntBre on 2025-10-14 16:33_

---

_Comment by @github-actions[bot] on 2025-10-14 16:55_

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

_Marked ready for review by @ntBre on 2025-10-14 16:55_

---

_Review requested from @MichaReiser by @ntBre on 2025-10-14 16:55_

---

_Review requested from @dhruvmanila by @ntBre on 2025-10-14 16:55_

---

_@amyreese approved on 2025-10-14 18:54_

LGTM

---

_@MichaReiser approved on 2025-10-15 09:47_

Thank you

---

_Merged by @ntBre on 2025-10-15 13:23_

---

_Closed by @ntBre on 2025-10-15 13:23_

---

_Branch deleted on 2025-10-15 13:23_

---
