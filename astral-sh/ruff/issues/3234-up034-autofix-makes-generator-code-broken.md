```yaml
number: 3234
title: UP034 autofix makes generator code broken
type: issue
state: closed
author: Olegt0rr
labels:
  - bug
assignees: []
created_at: 2023-02-26T09:00:48Z
updated_at: 2023-02-27T03:47:09Z
url: https://github.com/astral-sh/ruff/issues/3234
synced_at: 2026-01-12T15:54:43Z
```

# UP034 autofix makes generator code broken

---

_@Olegt0rr_

## 1. Let's take an example:

### Code:
```python
the_first_one = next(
    (some_integer for some_integer in range(1000) if some_integer // 2 == 0)
)
```

### Result:
```
0
```

It works.

## 2. Let's make ruff check

### Result:

```
UP034 [*] Avoid extraneous parentheses
COM812 [*] Trailing comma missing
```

## 3. Let's apply ruff check with the --fix option

### Result code:

```python
the_first_one = next(
    some_integer for some_integer in range(1000) if some_integer // 2 == 0,
)
```

### Run code result:

```python
  File "draft.py", line 2
    some_integer for some_integer in range(1000) if some_integer // 2 == 0,
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
SyntaxError: Generator expression must be parenthesized
```





---

_Label `bug` added by @charliermarsh on 2023-02-26 15:50_

---

_Comment by @charliermarsh on 2023-02-26 19:12_

I think this is a rare case in which applying two independent edits is causing a syntax error. It's fine to add the trailing comma as long as the parentheses are present, and it's fine to remove the parentheses as long as there's no trailing comma, but adding the trailing comma _and_ removing the parentheses is invalid.

---

_Comment by @matthewlloyd on 2023-02-26 19:40_

I tried switching the order of the checks but that doesn't help either. The second check doesn't see the edits made by the first. 

This does seem kind of important - as a user I would want autofixes to never break code. Since I thought about it a bit I'll share some possible solutions, though some are obvious I guess:

* Define a list of groups of mutually incompatible autofixes, to e.g. always disable the UP034 autofix when the COM812 autofix is enabled (not just when it has actually been applied, to preserve idempotence across Ruff runs).
* Divide the autofixable checks into passes (where each pass contains no mutually incompatible autofixes) and rerun the lexer in between each pass if autofixes have been applied.
* Have autofixes mark tokens as being removed, then ignore those tokens in the checkers.
* Have autofixes return a modified LexResult (or modify a mutable one) at the time the diagnostic is created.
* Teach UP034 to ignore situations where COM812 would apply when it's enabled, or vice versa.

---

_Comment by @charliermarsh on 2023-02-26 19:49_

Without addressing the broader issue, one hack we've used in the past is to expand the range of the autofix in some way, such that the fixes have conflicting edit ranges. If the edit ranges conflict, we'll only apply the first of the two fixes in the initial pass. Then, in the second pass, we wouldn't raise the second violation at all (hopefully) and would thus avoid the fix.

---

_Comment by @matthewlloyd on 2023-02-26 20:05_

Ah OK, nice - I didn't see that the linter already runs multiple passes :). That's easy then, and I have a working fix for this. Will submit a PR when I get back to my computer later today.

---

_Comment by @charliermarsh on 2023-02-26 20:06_

Yeah, I don't consider it a "good" solution but it does work.

---

_Closed by @charliermarsh on 2023-02-27 03:47_

---
