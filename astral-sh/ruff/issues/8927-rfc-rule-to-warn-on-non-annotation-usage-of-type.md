---
number: 8927
title: "RFC: rule to warn on non-annotation usage of TYPE_CHECKING imports"
type: issue
state: closed
author: RonnyPfannschmidt
labels:
  - question
assignees: []
created_at: 2023-11-30T16:48:20Z
updated_at: 2023-12-01T10:26:25Z
url: https://github.com/astral-sh/ruff/issues/8927
synced_at: 2026-01-10T01:22:48Z
---

# RFC: rule to warn on non-annotation usage of TYPE_CHECKING imports

---

_Issue opened by @RonnyPfannschmidt on 2023-11-30 16:48_

related to https://github.com/python/mypy/issues/16587

it would be great to warn if a variable that is imported using `if TYPE_CHECKING:` was used outside of a type anotation

aka cast/isinstance checks, calls)

---

_Comment by @flisboac on 2023-11-30 17:09_

As an additional flag/config, this would be very useful.

---

_Comment by @charliermarsh on 2023-11-30 17:10_

Can you try out [`TCH004`](https://docs.astral.sh/ruff/rules/runtime-import-in-type-checking-block/)?

---

_Comment by @charliermarsh on 2023-12-01 03:04_

It may be what you're looking for -- at least, that's the intent :)

---

_Label `question` added by @charliermarsh on 2023-12-01 03:04_

---

_Comment by @RonnyPfannschmidt on 2023-12-01 10:26_

indeed, the rule fits, i missed it when initially looking, thanks for the suggestion and sorry for the noise

---

_Closed by @RonnyPfannschmidt on 2023-12-01 10:26_

---
