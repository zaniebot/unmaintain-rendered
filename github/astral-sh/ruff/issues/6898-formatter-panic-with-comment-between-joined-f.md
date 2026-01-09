---
number: 6898
title: formatter panic with comment between joined f-strings
type: issue
state: closed
author: davidszotten
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-08-26T13:56:09Z
updated_at: 2023-09-01T16:27:34Z
url: https://github.com/astral-sh/ruff/issues/6898
synced_at: 2026-01-07T13:12:15-06:00
---

# formatter panic with comment between joined f-strings

---

_Issue opened by @davidszotten on 2023-08-26 13:56_

a3d4f08f moved the f-string comment handling added in f091b46 (which unfortunately didn't add enough test cases)

```
(
    f'{1}' # comment
    f'{2}'
)
```

panics with 

```
Unexpected token between nodes: `"' "`', crates/ruff_python_formatter/src/comments/placement.rs:122:17
```

---

_Label `bug` added by @MichaReiser on 2023-08-26 14:10_

---

_Label `formatter` added by @MichaReiser on 2023-08-26 14:10_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-26 14:10_

---

_Comment by @cnpryer on 2023-09-01 15:09_

What's the correct way to read 
```
`"' "`'
```

---

_Comment by @cnpryer on 2023-09-01 15:51_

I imagine the `"`s are just lexing noise?

https://github.com/astral-sh/ruff/blob/fbc9b5a604c98ab0e676a576a6e26be275efc364/crates/ruff_python_formatter/src/comments/placement.rs#L122-L126

Edit: *noise* probably isn't a good way to describe it. I mean it's lexed as `"` tokens or something?

---

_Comment by @charliermarsh on 2023-09-01 15:58_

I think the `SimpleTokenizer` intentionally doesn't support lexing strings, so it hits the first quote and then lexes any subsequent characters as bogus. I'm not quite familiar enough with the f-string comment handling to be sure how to solve this, but I can play around with it for a sec to get an idea.

---

_Comment by @davidszotten on 2023-09-01 15:58_

i believe the issue is that the preceding node is the `FormattedValue` _inside the f-string

the printed slice is 

```
 f'{1}' # comment
      ^^
```



---

_Comment by @davidszotten on 2023-09-01 16:00_

the previous solution was re-assigning the comment from the `FormattedValue` to the surrounding f-string

---

_Comment by @charliermarsh on 2023-09-01 16:00_

Yeah, makes sense. I can probably restore it.

---

_Comment by @davidszotten on 2023-09-01 16:03_

i found this when trying to test #6365 after rebasing on top of the refactor in the pr description. 

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-01 16:06_

---

_Referenced in [astral-sh/ruff#7047](../../astral-sh/ruff/pulls/7047.md) on 2023-09-01 16:12_

---

_Closed by @charliermarsh on 2023-09-01 16:27_

---

_Referenced in [astral-sh/ruff#20580](../../astral-sh/ruff/pulls/20580.md) on 2025-09-25 18:30_

---
