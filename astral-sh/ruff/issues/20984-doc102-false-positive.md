```yaml
number: 20984
title: "DOC102: false positive"
type: issue
state: open
author: mhindery
labels:
  - bug
  - docstring
assignees: []
created_at: 2025-10-20T07:26:42Z
updated_at: 2025-10-20T13:54:22Z
url: https://github.com/astral-sh/ruff/issues/20984
synced_at: 2026-01-10T11:10:00Z
```

# DOC102: false positive

---

_Issue opened by @mhindery on 2025-10-20 07:26_

### Summary

I noticed two false positives after upgrading ruff to 0.14.1 on the DOC rules:

Having a function like this:

```python
def foo(bar: int) -> bool:
    """
    Do something.
    Args:
        bar: some argument
    Returns:
        bool: some return value
    """
    return bar > 0
```

ruff raises two violations:

```shell
mathieu@Mac website % uv run ruff check --select DOC ~/Desktop/false_positive.py
DOC201 `return` is not documented in docstring
 --> /Users/mathieu/Desktop/false_positive.py:2:5
  |
1 |   def foo_1(bar: int) -> bool:
2 | /     """
3 | |     Do something.
4 | |     Args:
5 | |         bar: some argument
6 | |     Returns:
7 | |         bool: some return value
8 | |     """
  | |_______^
9 |       return bar > 0
  |
help: Add a "Returns" section to the docstring

DOC102 Documented parameter `bool` is not in the function's signature
 --> /Users/mathieu/Desktop/false_positive.py:7:9
  |
5 |         bar: some argument
6 |     Returns:
7 |         bool: some return value
  |         ^^^^
8 |     """
9 |     return bar > 0
  |
help: Remove the extraneous parameter from the docstring

Found 2 errors.
```

Both are a false positive, as the returns section is present and the signature is correct following the docstring. The issue lies with the whitespace that is missing before the returns section; after adding that it no longer flags. However the raised errors are wrong in my opinion, as both messages are not true.

---

_Comment by @ntBre on 2025-10-20 13:31_

Thanks for the report! Yeah, hopefully  we can do better with  our section detection here.

---

_Label `bug` added by @ntBre on 2025-10-20 13:31_

---

_Label `docstring` added by @ntBre on 2025-10-20 13:31_

---

_Comment by @mhindery on 2025-10-20 13:47_

I do like that it flags a bad docstring style/formatting, just not coincidently under the wrong category :)

---

_Comment by @ntBre on 2025-10-20 13:54_

I think the spacing should be caught by one of the D400 rules like [no-blank-line-before-section (D411)](https://docs.astral.sh/ruff/rules/no-blank-line-before-section/#no-blank-line-before-section-d411), even if we suppress these two DOC rules. But that will be a good thing to test!

---
