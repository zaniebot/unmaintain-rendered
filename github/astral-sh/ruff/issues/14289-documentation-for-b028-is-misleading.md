---
number: 14289
title: "(📚) documentation for `B028` is misleading"
type: issue
state: closed
author: KotlinIsland
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2024-11-12T01:09:29Z
updated_at: 2024-11-23T07:45:30Z
url: https://github.com/astral-sh/ruff/issues/14289
synced_at: 2026-01-07T13:12:16-06:00
---

# (📚) documentation for `B028` is misleading

---

_Issue opened by @KotlinIsland on 2024-11-12 01:09_

> The `warnings.warn` method uses a `stacklevel` of 1 by default, which limits the rendered stack trace to that of the line on which the `warn` method is called.

https://docs.astral.sh/ruff/rules/no-explicit-stacklevel/#no-explicit-stacklevel-b028

reading this, it implies that if a higher number is set, then the stack trace will contain multiple entries

> It's recommended to use a `stacklevel` of 2 or higher, give the caller more context about the warning.

```diff
- give
+ giving
OR
+ to give
```

### maybe

```
The `warnings.warn` method uses a stacklevel of 1 by default, which will output a stack frame of the line on which the `warn` method is called. Setting it to a higher number will output a stack frame from higher up the stack.

It's recommended to use a `stacklevel` of 2 or higher, to give the caller a more appropriate context about the warning.
```

---

_Label `documentation` added by @dylwil3 on 2024-11-12 03:15_

---

_Label `good first issue` added by @dylwil3 on 2024-11-12 03:15_

---

_Comment by @glthm on 2024-11-14 11:07_

Has this been included in one of your branches yet @dylwil3?

---

_Comment by @dylwil3 on 2024-11-14 13:11_

Nope! Hoping for a new contributor to take it as an excuse to pull down the repo and get comfortable with the PR process 😄

---

_Comment by @glthm on 2024-11-14 14:56_

Yeah I was starting to look at it but the PR process is a bit complex so I won't be able to get into it during this month, so anyone can feel free to do it

---

_Referenced in [astral-sh/ruff#14338](../../astral-sh/ruff/pulls/14338.md) on 2024-11-14 15:44_

---

_Comment by @njhearp on 2024-11-14 15:48_

I thought I would try this to learn the process for a pull request. Sorry about the mess with the commits.

---

_Comment by @dylwil3 on 2024-11-14 15:59_

- @glthm Please don't hesitate to reach out and I can help navigate the PR process! Also would love feedback if the CONTRIBUTING docs could be made more helpful / less intimidating 😄 

- Thanks @njhearp! No need to apologize for the commits - great contribution!

---

_Closed by @dylwil3 on 2024-11-23 07:45_

---
