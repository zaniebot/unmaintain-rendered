---
number: 7595
title: First string token is always considered as docstring
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - docstring
assignees: []
created_at: 2023-09-22T10:05:36Z
updated_at: 2024-04-14T18:14:13Z
url: https://github.com/astral-sh/ruff/issues/7595
synced_at: 2026-01-07T13:12:15-06:00
---

# First string token is always considered as docstring

---

_Issue opened by @dhruvmanila on 2023-09-22 10:05_

In the [docstring detection state machine](https://github.com/astral-sh/ruff/blob/82978ac9b54e16286e5b2fa60240e06ad8cc88d7/crates/ruff_linter/src/lex/docstring_detection.rs), the first string token is always considered as docstring without considering the context. For example, the first strings in the following example are considered as docstrings:

```python
"abcba".strip("ab")
```

On the opposite end, if multiple strings are implicitly concatenated then only the first string is considered as docstring:

```python
"first" "second"
```

Python considers the entire string (`"firststring"`) as the module docstring but the state machine will only say that `"first"` is docstring while `"second"` is a normal string.

---

_Label `bug` added by @dhruvmanila on 2023-09-22 10:05_

---

_Label `docstring` added by @dhruvmanila on 2023-09-22 10:05_

---

_Comment by @charliermarsh on 2023-09-29 02:03_

Yeah, we need something more reliable here. I wish this could use our AST-based docstring detection -- that is, I wish we could just move these rules to the AST checker.

We _could_ move them, but we'd then have to re-lex the strings in the AST for any implicit concatenations, which seems expensive.

---

_Comment by @MichaReiser on 2023-09-29 08:28_

> We could move them, but we'd then have to re-lex the strings in the AST for any implicit concatenations, which seems expensive.

Why is this necessary, considering that python detects the whole string as docsttring? 

Let's see if @dhruvmanila ends up refactoring our string representation anyway to represent string parts in the AST.

---

_Comment by @charliermarsh on 2023-09-29 17:22_

@MichaReiser -- Because the string could contain comments in-between the segments:

```python
x = (
    "foo" # comment
    "bar"
)
```

This isn't a problem in the token-based model because we iterate over string segments and comment tokens. But in the AST, we'd need to delineate the string contents from the comments.

---

_Comment by @MichaReiser on 2023-11-03 01:46_

> This isn't a problem in the token-based model because we iterate over string segments and comment tokens. But in the AST, we'd need to delineate the string contents from the comments.

I see, but we would only need to do this for strings where implicit concatenated is true. So it shouldn't be as many? But I guess it is now more complicated with the `fstrings` support. 

---

_Comment by @dhruvmanila on 2023-11-06 04:17_

I haven't explored it fully but I don't think it'll be an issue once the new AST node for implicit string concatenation is complete.

---

_Comment by @charliermarsh on 2023-12-07 02:18_

I think we can now solve this.

---

_Comment by @dhruvmanila on 2023-12-07 02:40_

Yeah, I can look at it tonight, looks interesting :)

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-12-07 02:41_

---

_Comment by @dhruvmanila on 2023-12-07 04:32_

I think we would need to move the existing rules which uses the state machine to be based on AST instead of tokens. There aren't many -- `RUF001-003` and `flake8-quotes` rules. I think the only reason they're token based is because of implicit string concatenation.

The implementation should probably be in the semantic model.

---

_Comment by @charliermarsh on 2023-12-07 04:39_

Yeah ideally we would get rid of that state machine entirely.

---

_Referenced in [astral-sh/ruff#9960](../../astral-sh/ruff/pulls/9960.md) on 2024-02-12 19:09_

---

_Referenced in [astral-sh/ruff#9993](../../astral-sh/ruff/pulls/9993.md) on 2024-02-15 07:13_

---

_Referenced in [astral-sh/ruff#10312](../../astral-sh/ruff/pulls/10312.md) on 2024-03-09 12:03_

---

_Referenced in [astral-sh/ruff#10548](../../astral-sh/ruff/pulls/10548.md) on 2024-03-24 17:27_

---

_Comment by @charliermarsh on 2024-04-12 01:43_

@dhruvmanila - I can't remember, can we close this?

---

_Comment by @dhruvmanila on 2024-04-12 03:04_

No, the last rule (`Q003`) is remaining, maybe I'll pick that up this weekend. I remember that was a bit difficult because it supports PEP 701 syntax.

---

_Referenced in [astral-sh/ruff#10923](../../astral-sh/ruff/pulls/10923.md) on 2024-04-14 09:34_

---

_Closed by @dhruvmanila on 2024-04-14 18:14_

---
