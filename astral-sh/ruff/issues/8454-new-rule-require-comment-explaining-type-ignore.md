```yaml
number: 8454
title: "new rule - require comment explaining `type:ignore`/`pyright:ignore` comments"
type: issue
state: open
author: DetachHead
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-11-03T01:25:20Z
updated_at: 2024-04-08T23:43:13Z
url: https://github.com/astral-sh/ruff/issues/8454
synced_at: 2026-01-10T11:09:50Z
```

# new rule - require comment explaining `type:ignore`/`pyright:ignore` comments

---

_Issue opened by @DetachHead on 2023-11-03 01:25_

it would be nice to have a rule that enforces that `type:ignore` comments have a comment above them describing the reason the error is being suppressed, eg:

incorrect:
```py
def foo():
    a: str = 1  # type:ignore[assignment] # error: add a comment on the line above explaining why the error is being ignored
```

correct:
```py
def foo():
    # false positive, see https://github.com/python/mypy/issues/1
    a: str = 1  # type:ignore[assignment]
```

eslint has a rule for this: https://typescript-eslint.io/rules/ban-ts-comment/

related: #5182

---

_Label `rule` added by @charliermarsh on 2023-11-03 03:51_

---

_Label `needs-decision` added by @charliermarsh on 2023-11-03 03:51_

---

_Comment by @Avasam on 2024-04-08 23:35_

Does this request extend to checker-specific suppression comments as well? (`# pyright: ignore` is the only one I can think of, since `# noqa` is requested here: https://github.com/astral-sh/ruff/issues/5182)

---

_Renamed from "new rule - require comment explaining `type:ignore` comments" to "new rule - require comment explaining `type:ignore`/`pyright:ignore` comments" by @DetachHead on 2024-04-08 23:41_

---

_Comment by @DetachHead on 2024-04-08 23:43_

true, thanks. i updated the issue

---
