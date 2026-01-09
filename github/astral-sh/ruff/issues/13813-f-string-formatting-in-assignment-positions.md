---
number: 13813
title: F-String formatting in assignment positions
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
created_at: 2024-10-18T14:19:50Z
updated_at: 2024-11-26T09:37:19Z
url: https://github.com/astral-sh/ruff/issues/13813
synced_at: 2026-01-07T13:12:16-06:00
---

# F-String formatting in assignment positions

---

_Issue opened by @MichaReiser on 2024-10-18 14:19_

The new f-string formatting in assignment-value positions seems inconsistent to me:

```python
aaaaaaaaaaaaaaaaaa = f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{
    expression}moreeeeeeeeeeeeeeeee"
```

Gets formatted to:

```python
aaaaaaaaaaaaaaaaaa = f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{
    expression
}moreeeeeeeeeeeeeeeee"
```

Which avoids parentheses but it doesn't feel like the ideal formatting and it doesn't match the formatting if the expression starts out "flat"

```python
aaaaaaaaaaaaaaaaaa = (
    f"testeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee{expression}moreeeeeeeeeeeeeeeee"
)
```



---

_Label `formatter` added by @MichaReiser on 2024-10-18 14:19_

---

_Label `preview` added by @MichaReiser on 2024-10-18 14:19_

---

_Referenced in [astral-sh/ruff#13371](../../astral-sh/ruff/issues/13371.md) on 2024-10-18 14:20_

---

_Comment by @MichaReiser on 2024-10-18 15:29_

Hmm, I might have to fix this for the implicit concatenated string formatting because it handles this correctly and this now results in instable formatting

---

_Comment by @MichaReiser on 2024-10-18 15:30_

One challenge is that we don't want to inline the comments if the expression breaks:

```python
a = f"test{
    expression
}flat" f"can be {
    joined
} togethereeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee" # inline
```

Should be formatted to 

```python
a = (
    f"test{expression}flatcan be {
        joined
    } togethereeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee"
)   # inline
``` 

and not 

```python

```python
a = (
    f"test{expression}flatcan be {
        joined
    } togethereeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee"  # inline
)
```

---

_Comment by @dhruvmanila on 2024-10-23 03:51_

> Hmm, I might have to fix this for the implicit concatenated string formatting because it handles this correctly and this now results in instable formatting

@MichaReiser Just want to check-in whether you ended up fixing this with implicit concatenated string formatting?

---

_Comment by @MichaReiser on 2024-10-23 05:48_

I fixed the instability but I didn't fix the formatting. I suggest waiting with this task until the implicit concatenated string formatting PR is merged or that you work on top of it. We'll otherwise end up with horrible merge conflicts

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-11-15 09:35_

---

_Referenced in [astral-sh/ruff#14454](../../astral-sh/ruff/pulls/14454.md) on 2024-11-19 13:40_

---

_Closed by @dhruvmanila on 2024-11-26 09:37_

---
