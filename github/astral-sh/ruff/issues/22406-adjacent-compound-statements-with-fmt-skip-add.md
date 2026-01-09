---
number: 22406
title: "Adjacent compound statements with `fmt: skip` add extraneous empty lines"
type: issue
state: closed
author: dylwil3
labels:
  - formatter
  - style
assignees: []
created_at: 2026-01-05T18:41:27Z
updated_at: 2026-01-06T15:09:57Z
url: https://github.com/astral-sh/ruff/issues/22406
synced_at: 2026-01-07T13:12:16-06:00
---

# Adjacent compound statements with `fmt: skip` add extraneous empty lines

---

_Issue opened by @dylwil3 on 2026-01-05 18:41_

### Summary

This:

```
def a(): foo() # fmt: skip
def a(): foo() # fmt: skip
```

gets formatted to:

```
def a(): foo()  # fmt: skip


def a(): foo()  # fmt: skip
```

which looks a bit silly. This also deviates from Black's formatting.

I suppose an argument could be made that this is not a bug and we have to use `fmt: on/off` here since the rules for empty lines are coming from general rules about formatting function definitions in suites? But it feels like the output is unexpected for a generic user - it was certainly unexpected to me.


[Playground link](https://play.ruff.rs/a7192280-d8fd-432a-baac-d58dbc72f934)

### Version

_No response_

---

_Label `bug` added by @dylwil3 on 2026-01-05 18:41_

---

_Label `formatter` added by @dylwil3 on 2026-01-05 18:41_

---

_Referenced in [astral-sh/ruff#22405](../../astral-sh/ruff/pulls/22405.md) on 2026-01-05 18:41_

---

_Referenced in [astral-sh/ruff#22119](../../astral-sh/ruff/pulls/22119.md) on 2026-01-05 22:03_

---

_Comment by @MichaReiser on 2026-01-06 08:47_

I think this is fine. `fmt: skip` disables the formatting of the statement itself but not the formatting of its suroundings (including leading and trailing comments)

---

_Label `bug` removed by @MichaReiser on 2026-01-06 08:48_

---

_Label `style` added by @MichaReiser on 2026-01-06 08:48_

---

_Comment by @dylwil3 on 2026-01-06 15:09_

Fair enough!

---

_Closed by @dylwil3 on 2026-01-06 15:09_

---
