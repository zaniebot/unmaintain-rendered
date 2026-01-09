---
number: 11278
title: Support task-tags in flake8-todo rules
type: issue
state: open
author: benblank
labels:
  - configuration
assignees: []
created_at: 2024-05-04T04:39:29Z
updated_at: 2024-05-06T14:05:53Z
url: https://github.com/astral-sh/ruff/issues/11278
synced_at: 2026-01-07T13:12:15-06:00
---

# Support task-tags in flake8-todo rules

---

_Issue opened by @benblank on 2024-05-04 04:39_

It would be nice if the flake8-todo rules affecting TODO formatting (TD002-7) supported the [`lint.task-tags` setting](https://docs.astral.sh/ruff/settings/#lint_task-tags) rather than just the hard-coded "directives" supported by the original rules in flake8-todo. As it stands, there's no way to add enforcement for custom tags, which would be helpful for teams which use them and want to make sure they're used consistently. It also "incorrectly" flags my maybe-todo comments (e.g. `# TODO?: switch to using fooble instead`) as missing a colon. 🙂

---

_Label `configuration` added by @charliermarsh on 2024-05-04 13:42_

---

_Comment by @charliermarsh on 2024-05-04 13:43_

Do you mean that TD001 should flag tags other than `FIXME` and `XXX`? (https://docs.astral.sh/ruff/rules/invalid-todo-tag/)

Or that rules like TD002 should _allow_ anything in `task-tags` (e.g., `FIXME(foo)` should be ok?), and enforce authorship on them? (https://docs.astral.sh/ruff/rules/missing-todo-author/)

---

_Label `question` added by @charliermarsh on 2024-05-04 13:43_

---

_Comment by @benblank on 2024-05-05 17:48_

Ah! You're right, TD001 doesn't make sense that way; my mistake, I'll fix the description.

No, I was indeed referring to e.g. being able to require `# TECHDEBT(author)` instead of just `# TECHDEBT` if TD002 is selected or, in my case, `# TODO?: some rationale` instead of just `# TODO?` with TD004, TD005, and TD007.

I think being able to keep track of when task tags are being used inconsistently would be a "nice to have" feature for projects which use multiple tags with different meanings.

---

_Label `question` removed by @charliermarsh on 2024-05-06 14:05_

---

_Referenced in [astral-sh/ruff#5407](../../astral-sh/ruff/issues/5407.md) on 2025-02-07 14:12_

---
