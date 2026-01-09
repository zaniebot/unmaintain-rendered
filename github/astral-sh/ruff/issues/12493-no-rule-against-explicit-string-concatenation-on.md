---
number: 12493
title: No rule against explicit string concatenation on same line
type: issue
state: closed
author: DaniBodor
labels:
  - configuration
assignees: []
created_at: 2024-07-24T15:51:22Z
updated_at: 2024-07-25T06:19:13Z
url: https://github.com/astral-sh/ruff/issues/12493
synced_at: 2026-01-07T13:12:15-06:00
---

# No rule against explicit string concatenation on same line

---

_Issue opened by @DaniBodor on 2024-07-24 15:51_

There is currently no rule that flags explicit string concatenation if it occurs on a single line, even though this makes for very ugly code.

Explicit string concatenations are flagged by ISC003 when they are on separate lines (example 1) and implicit string concatenation on a single line is flagged by ISC001 (example 2). 
I would like example 3 below to also be picked up by ruff, either in combination with one of the rules mentioned, or by a separate rule altogether.
This is especially relevant, given that example 1 is converted into example 3 when running `ruff format`. This led to a code base that I was trying to clean up now being riddled with explicit one-line string concatenations.

```py
example_1 = (
    "The quick brown fox jumps over the lazy "
    + "dog"
)

example_2 = "The quick brown fox jumps over the lazy " "dog"

example_3 = "The quick brown fox jumps over the lazy " + "dog"
```

running ruff:

```bash
$ ruff check --select ALL --output-format concise --ignore D
string_concatenations.py:2:5: ISC003 Explicitly concatenated string should be implicitly concatenated
string_concatenations.py:6:13: ISC001 [*] Implicitly concatenated string literals on one line
Found 2 errors.
[*] 1 fixable with the `--fix` option.
```

**EDITED** for more clarity

---

_Comment by @MichaReiser on 2024-07-24 19:06_

> Explicit string concatenations are flagged by ISC003 when they are on separate lines, but no error message flags explicit string concatenation on single lines.

I think that's in the rule's spirit. From the documentation:

> For string literals that wrap across multiple lines, implicit string concatenation within parentheses is preferred over explicit concatenation using the + operator, as the former is more readable.

---

_Comment by @DaniBodor on 2024-07-24 21:21_

My issue is that there is no rule that flags this situation `"The quick brown fox jumps over the lazy " + "dog"`, even I believe that is something that should be avoided. I will clarify this in my original post.

---

_Label `configuration` added by @charliermarsh on 2024-07-24 21:21_

---

_Renamed from "Explicit string concatenation on same line is not flagged, while this is seen as incorrect usage when on multiple lines." to "No rule against explicit string concatenation on same line" by @DaniBodor on 2024-07-24 21:33_

---

_Comment by @MichaReiser on 2024-07-25 06:19_

Thanks for the clarification. I now understand what you're looking for. 

I'm sorry, but we decided to hold off on introducing new rules that conflict with our formatter because it leads to poor experience when the formatter and linter disagree (it's a configuration hazard). 

---

_Closed by @MichaReiser on 2024-07-25 06:19_

---
