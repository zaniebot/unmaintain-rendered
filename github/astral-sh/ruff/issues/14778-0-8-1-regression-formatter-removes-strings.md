---
number: 14778
title: "0.8.1 regression - formatter removes strings causing a syntax error on multiline fstring with implicitly concatenated strings and an `if` expression inside it"
type: issue
state: closed
author: DetachHead
labels:
  - bug
  - formatter
  - preview
assignees: []
created_at: 2024-12-04T23:04:14Z
updated_at: 2024-12-06T12:01:06Z
url: https://github.com/astral-sh/ruff/issues/14778
synced_at: 2026-01-07T13:12:16-06:00
---

# 0.8.1 regression - formatter removes strings causing a syntax error on multiline fstring with implicitly concatenated strings and an `if` expression inside it

---

_Issue opened by @DetachHead on 2024-12-04 23:04_

before:
```py
f"""{'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa' 'a' if True else ""}\
a"""
```

after:
```py
f"""{ if True else ""}\
a"""
```

[playground](https://play.ruff.rs/53e2a577-7898-4ac9-9ceb-c5c0c26eca36)

---

_Label `formatter` added by @AlexWaygood on 2024-12-05 01:10_

---

_Comment by @dhruvmanila on 2024-12-05 04:03_

A bit more minimal example:
```python
f"{'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa' 'a' if True else ""}"
```

If you remove one `a` from the string, the formatting is correct because it now fits on the line.

---

_Label `bug` added by @dhruvmanila on 2024-12-05 04:03_

---

_Label `preview` added by @dhruvmanila on 2024-12-05 04:03_

---

_Referenced in [astral-sh/ruff#13371](../../astral-sh/ruff/issues/13371.md) on 2024-12-05 06:57_

---

_Comment by @MichaReiser on 2024-12-05 07:35_

I think this is related to https://github.com/astral-sh/ruff/pull/14489

We may need a more local fix or change our approach in the f-string formatter to instead format the inner expression on its own with line-length set to max. 

---

_Assigned to @MichaReiser by @MichaReiser on 2024-12-06 09:42_

---

_Referenced in [astral-sh/ruff#14811](../../astral-sh/ruff/pulls/14811.md) on 2024-12-06 10:27_

---

_Closed by @MichaReiser on 2024-12-06 12:01_

---

_Closed by @MichaReiser on 2024-12-06 12:01_

---
