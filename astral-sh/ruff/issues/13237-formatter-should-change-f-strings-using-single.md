```yaml
number: 13237
title: "formatter should change f strings using single quotes with nested double quotes to use double quotes when `target-version` is `py312`"
type: issue
state: closed
author: DetachHead
labels:
  - bug
  - formatter
  - preview
assignees: []
created_at: 2024-09-04T04:19:26Z
updated_at: 2024-10-23T05:57:55Z
url: https://github.com/astral-sh/ruff/issues/13237
synced_at: 2026-01-12T15:54:52Z
```

# formatter should change f strings using single quotes with nested double quotes to use double quotes when `target-version` is `py312`

---

_@DetachHead_

on older python versions, the following code is a syntax error:
```py
f"{""}"
```
which is why the formatter does not convert the following f string to use double quotes:

```py
f'{""}'
```

but on python 3.12, it's allowed, so the formatter should always convert them to double quotes. [playground](https://play.ruff.rs/5435f8ea-ad9b-4d5f-8900-7c40c6b7a3fa)

---

_Comment by @dhruvmanila on 2024-09-04 04:25_

This is available under preview style (https://github.com/astral-sh/ruff/discussions/9785) although it seems like the formatter still doesn't change the outer quote. For example, if the quotes are inverted (`f"{''}"`), the formatter changes the inner quote to use double quotes. This case (`f'{""}'`) seems like a bug.

---

_Label `bug` added by @dhruvmanila on 2024-09-04 04:25_

---

_Label `formatter` added by @dhruvmanila on 2024-09-04 04:25_

---

_Label `preview` added by @dhruvmanila on 2024-09-04 04:25_

---

_Closed by @MichaReiser on 2024-10-23 05:57_

---
