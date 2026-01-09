---
number: 13358
title: Doctest dynamic line width is off if examples are indented
type: issue
state: closed
author: tugrulates
labels:
  - bug
  - formatter
  - linter
assignees: []
created_at: 2024-09-15T20:27:56Z
updated_at: 2024-09-27T07:09:08Z
url: https://github.com/astral-sh/ruff/issues/13358
synced_at: 2026-01-07T13:12:15-06:00
---

# Doctest dynamic line width is off if examples are indented

---

_Issue opened by @tugrulates on 2024-09-15 20:27_

Thank you for the great tool. I am encountering an issue with the docstring code formatter in `dynamic` mode.

When the doctest code has an indent that is more than the docstring indent, the dynamic calculation is off, and as a result, the formatter and the linter are in a disagreement.

I have searched the issue using "doctest", "docstring", "dynamic" keywords. #9126 is for the bug where this calculation if off for the prompt. The current issue is for when the prompt is extra indented.

```py
"""example.py file."""


def length(numbers: list[int]) -> int:
    """Get the length of the given list of numbers.

    Args:
        numbers: List of numbers.

    Returns:
        Integer length of the list of numbers.

    Example:
        >>> length([1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20])
        20
    """
    return len(numbers)

```

```toml
# pyproject.toml

[tool.ruff.format]
docstring-code-format = true

[tool.ruff.lint]
select = ['D', 'E']

[tool.ruff.lint.pydocstyle]
convention = "google"

```

```console
$ ruff --version
ruff 0.6.4
$ ruff format example.py 
1 file left unchanged
$ ruff check example.py 
example.py:14:89: E501 Line too long (91 > 88)
   |
13 |     Example:
14 |         >>> length([1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20])
   |                                                                                         ^^^ E501
15 |         20
16 |     """
   |

Found 1 error.
```

I am happy to take a stab at this.

---

_Label `incompatibility` added by @MichaReiser on 2024-09-16 09:25_

---

_Label `formatter` added by @MichaReiser on 2024-09-16 09:26_

---

_Label `linter` added by @MichaReiser on 2024-09-16 09:26_

---

_Label `great writeup` added by @MichaReiser on 2024-09-16 09:41_

---

_Comment by @MichaReiser on 2024-09-16 09:56_

Thanks for the great write-up.

Unfortunately, fixing this will be difficult because the pycodestyle linter doesn't support parsing code blocks. This also requires introducing a new setting to be compatible with the formatter (or we pick up the setting from the formatter?)

* Add a new `pycodestyle.max-doc-code-line-length` that defaults to `dynamic` (same as the `format.docstring-code-length` setting) if left unspecified
* Recognize code blocks in the `pycodestyle` linter and apply the dynamic or "fixed" length depending on the `max-doc-code-line-length` setting. 



---

_Label `help wanted` added by @MichaReiser on 2024-09-16 09:56_

---

_Comment by @MichaReiser on 2024-09-16 12:21_

Ignore my previous comment. I'm wrong. The `dynamic` setting actually ensures that this isn't a problem. Let me double  check why the list isn't handled correctly

---

_Label `incompatibility` removed by @MichaReiser on 2024-09-16 12:33_

---

_Label `great writeup` removed by @MichaReiser on 2024-09-16 12:33_

---

_Label `bug` added by @MichaReiser on 2024-09-16 12:33_

---

_Comment by @MichaReiser on 2024-09-16 12:45_

I added a few debug statements to the code to understand the values calculated in 

https://github.com/astral-sh/ruff/blob/138e70bd5c01d61061247c43b1bbb33d0290a18f/crates/ruff_python_formatter/src/string/docstring.rs#L499-L508

```
[crates/ruff_python_formatter/src/string/docstring.rs:500:41] self.f.options().line_width().value() = 88
[crates/ruff_python_formatter/src/string/docstring.rs:501:36] self.f.options().indent_width() = IndentWidth(
    4,
)
[crates/ruff_python_formatter/src/string/docstring.rs:502:36] self.f.context().indent_level() = IndentLevel {
    level: 2,
}
[crates/ruff_python_formatter/src/string/docstring.rs:505:37] kind.extra_indent_ascii_spaces() = 4
[crates/ruff_python_formatter/src/string/docstring.rs:524:30] line_width = LineWidth(
    80,
)
```

`indent_level.to_ascii_spaces(indent_width)` reduces the indent level by 1. That makes me think that the problem here is that the formatter doesn't account for the indentation compared to `Example`.

The new logic has to account for that the formatter normalizes the indentation when some lines are incorrectly indented. I think that's done in https://github.com/astral-sh/ruff/blob/138e70bd5c01d61061247c43b1bbb33d0290a18f/crates/ruff_python_formatter/src/string/docstring.rs#L380-L471

@tugrulates do you want to take a stab at this?

---

_Comment by @tugrulates on 2024-09-16 15:10_

Certainly, I can work on this.

---

_Assigned to @tugrulates by @MichaReiser on 2024-09-16 15:41_

---

_Referenced in [astral-sh/ruff#13371](../../astral-sh/ruff/issues/13371.md) on 2024-09-16 16:19_

---

_Comment by @MichaReiser on 2024-09-26 13:10_

I think I have a fix for this

---

_Referenced in [astral-sh/ruff#13523](../../astral-sh/ruff/pulls/13523.md) on 2024-09-26 13:29_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-09-26 15:24_

---

_Unassigned @tugrulates by @MichaReiser on 2024-09-26 15:24_

---

_Label `help wanted` removed by @MichaReiser on 2024-09-26 15:24_

---

_Closed by @MichaReiser on 2024-09-27 07:09_

---

_Referenced in [astral-sh/ruff#14984](../../astral-sh/ruff/issues/14984.md) on 2024-12-15 14:44_

---
