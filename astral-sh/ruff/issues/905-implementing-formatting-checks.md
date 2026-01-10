---
number: 905
title: Implementing % formatting checks
type: issue
state: closed
author: olliemath
labels:
  - rule
assignees: []
created_at: 2022-11-25T16:09:17Z
updated_at: 2022-11-28T02:30:57Z
url: https://github.com/astral-sh/ruff/issues/905
synced_at: 2026-01-10T01:22:38Z
---

# Implementing % formatting checks

---

_Issue opened by @olliemath on 2022-11-25 16:09_

Now that I've implemented the `.format` checks, I feel like I should grind out the `%` format checks too. Although I won't have time for the implementation until the back half of next week.

The strategy would be similar: if we detect a binary operation that looks right, build a summary of the format string and use that to perform subsequent checks.

Most of the logic is going to be in parsing the string literal. We could port pyflakes' own implementation https://github.com/PyCQA/pyflakes/blob/main/pyflakes/checker.py#L94, which is mostly regex. Or we could try and see how rustpython is doing this.

I'm unsure where exactly to look in the rustpython codebase for this, I'm guessing it's either hard-coded to be called when processing the binop within the interpreter, or in the definition of the string type itself. Need to do a bit of research to see if it's accessible, or tied up with python internals.

Does that seem like a good plan? Any ideas on tracking down the parsing logic in rustpython?

Like I say, I probably won't be able to get around to this for a week or so, so if anybody else would like to pick it up in the meantime, don't let me stop you!

---

_Comment by @olliemath on 2022-11-25 16:46_

I think I've tracked down the place to start tugging at the RustPython codebase: https://github.com/RustPython/RustPython/blob/4331d652853a8618bc634b2f7f96878640f463a3/vm/src/anystr.rs#L438 it leads us to `CFormatString` in `vm/src/cformat.rs`.

---

_Label `rule` added by @charliermarsh on 2022-11-25 22:12_

---

_Referenced in [astral-sh/ruff#919](../../astral-sh/ruff/pulls/919.md) on 2022-11-27 02:57_

---

_Closed by @charliermarsh on 2022-11-28 02:30_

---
