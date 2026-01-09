---
number: 11323
title: "[Rule Request] [TD] No whitespace after TODO - \"TODO     (Author)\" -> \"TODO(Author)\""
type: issue
state: open
author: david-fischer
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-05-07T10:53:07Z
updated_at: 2024-05-10T05:49:04Z
url: https://github.com/astral-sh/ruff/issues/11323
synced_at: 2026-01-07T13:12:15-06:00
---

# [Rule Request] [TD] No whitespace after TODO - "TODO     (Author)" -> "TODO(Author)"

---

_Issue opened by @david-fischer on 2024-05-07 10:53_

Hi ruff-team,

I noticed that the TD-rules do **not** get triggered on arbitrary many spaces between TODO and author:
```python
def function_with_todo() -> None:
    return  # TODO              (Test): This does not trigger any TD rule.
    # https://some.link
```

I would find it easier to search the codebase for TODOs related to a given person if I would not need to include the possibility of whitespace in the regex. Hence I would suggest to autofix this to `# TODO(Test):...`.

Thanks for all your great work! 

---

_Label `rule` added by @dhruvmanila on 2024-05-10 05:49_

---

_Label `needs-decision` added by @dhruvmanila on 2024-05-10 05:49_

---
