---
number: 9509
title: "Add a rule for `defaultdict(default_factory=list)`"
type: issue
state: closed
author: hauntsaninja
labels:
  - rule
  - accepted
assignees: []
created_at: 2024-01-14T04:11:05Z
updated_at: 2024-01-29T03:58:37Z
url: https://github.com/astral-sh/ruff/issues/9509
synced_at: 2026-01-10T01:22:49Z
---

# Add a rule for `defaultdict(default_factory=list)`

---

_Issue opened by @hauntsaninja on 2024-01-14 04:11_

Kwargs in the defaultdict constructor are forwarded to the dict constructor, so this creates a defaultdict with `{"default_factory": list}`. Since this is a valid call, type checkers may or may not catch this (and even when they catch it, the error might be a little confusing). ruff could lint this specific situation and mention use of `collections.defaultdict(list)` instead.

---

_Renamed from "Add a rule for `collections.defaultdict(default_factory=list)`" to "Add a rule for `defaultdict(default_factory=list)`" by @hauntsaninja on 2024-01-14 04:11_

---

_Label `rule` added by @charliermarsh on 2024-01-14 15:14_

---

_Label `accepted` added by @charliermarsh on 2024-01-14 15:14_

---

_Comment by @charliermarsh on 2024-01-14 15:14_

Seems like a good rule.

---

_Comment by @Skylion007 on 2024-01-14 15:48_

@hauntsaninja You should also open the issue in the flake8-bugbear repo, they may be interested in it.

---

_Comment by @mikeleppane on 2024-01-24 18:45_

I can take this. 

---

_Assigned to @mikeleppane by @charliermarsh on 2024-01-24 19:37_

---

_Comment by @mikeleppane on 2024-01-25 21:01_

@hauntsaninja did you use 'list' as an example or I assume this applies to any callable? I tried default_factory with mypy 1.7.1 and it truly seems to be confused with different input types (list, int, dict, lambda, ...).

---

_Comment by @hauntsaninja on 2024-01-25 21:05_

As an example! :-) Like I said, type checkers can't catch the call itself since it's valid. They may be able to surface an issue later because they'll infer a different type for the defaultdict.

---

_Referenced in [astral-sh/ruff#9651](../../astral-sh/ruff/pulls/9651.md) on 2024-01-26 15:03_

---

_Comment by @charliermarsh on 2024-01-29 03:58_

Added in https://github.com/astral-sh/ruff/pull/9651, thanks @mikeleppane!

---

_Closed by @charliermarsh on 2024-01-29 03:58_

---
