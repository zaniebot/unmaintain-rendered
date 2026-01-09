---
number: 18435
title: Please add a lint that is the opposite of redundant-open-modes (UP015)
type: issue
state: open
author: zackw
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-06-02T20:16:53Z
updated_at: 2025-06-17T19:35:38Z
url: https://github.com/astral-sh/ruff/issues/18435
synced_at: 2026-01-07T13:12:16-06:00
---

# Please add a lint that is the opposite of redundant-open-modes (UP015)

---

_Issue opened by @zackw on 2025-06-02 20:16_

### Summary

The UP015 lint wants you to change `open(path, "r")` into just `open(path)`.

Preferred code style in my neck of the woods is exactly the opposite: one should _always_ specify the mode argument to `open`; moreover, one should _always_ specify text or binary.  So I would like there to be a lint that suggests changing both `open(path)` and `open(path, "r")` to `open(path, "rt")`.

---

_Label `rule` added by @ntBre on 2025-06-02 20:32_

---

_Label `needs-decision` added by @ntBre on 2025-06-02 20:32_

---

_Comment by @ntBre on 2025-06-02 20:34_

I can see how this could be preferable, but I think we generally try to avoid adding conflicting rules. We'd need to figure out how to reconcile that here.

---

_Comment by @zackw on 2025-06-02 20:44_

That's a sensible goal to have.

Being new to this project, I don't know how similar things have been handled in the past. I know pycodestyle has (or at least used to have) [W503](https://www.flake8rules.com/rules/W503.html) and [W504](https://www.flake8rules.com/rules/W504.html) which are mutually contradictory, so neither of them is on by default and you have to pick one, but it looks like ruff absorbed neither of them...?

---

_Comment by @ntBre on 2025-06-02 20:56_

We definitely have some that conflict. You can see them with `--select all`, for example:

```shell
$ ruff check --select ALL --no-cache --output-format=concise
warning: `incorrect-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `incorrect-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
```

We just have to decide if we want to add more.

Not totally related, but I also found a previous discussion of W503 and W504 [here](https://github.com/astral-sh/ruff/issues/4125).

---

_Comment by @Avasam on 2025-06-03 16:52_

I agree I prefer a rule to be configurable rather than contradicting. (Especially for extending configuration).

`open-mode` could be a rule configured to be "implicit or explicit"

A configurable `UP015` could act like pyupgrade by default.

---
