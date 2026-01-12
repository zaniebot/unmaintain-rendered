```yaml
number: 8453
title: new rule - require description explaining noqa comments
type: issue
state: closed
author: DetachHead
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-11-03T01:22:25Z
updated_at: 2023-11-03T12:31:26Z
url: https://github.com/astral-sh/ruff/issues/8453
synced_at: 2026-01-12T15:54:48Z
```

# new rule - require description explaining noqa comments

---

_@DetachHead_

noqa comments are not very descriptive, especially since they don't support the more readable name and instead only allow you to specify the code (related: #1773)

it would be nice to have a rule that enforces that noqa comments have a comment above them describing the reason the error is being suppressed, eg:

incorrect:
```py
def foo():
    a = 1  # noqa: F841 # error: add a comment on the line above explaining why the error is being ignored
```

correct:
```py
def foo():
    # false positive, see https://github.com/astral-sh/ruff/issues/8453
    a = 1  # noqa: F841
```

eslint has a rule for this: https://eslint-community.github.io/eslint-plugin-eslint-comments/rules/require-description.html 

---

_Comment by @KotlinIsland on 2023-11-03 01:24_

Extremely valuable for long term project maintenance

---

_Label `rule` added by @charliermarsh on 2023-11-03 03:52_

---

_Label `needs-decision` added by @charliermarsh on 2023-11-03 03:52_

---

_Comment by @tjkuson on 2023-11-03 11:15_

This might be related to #5182 where the rule idea seems to be blocked by the lack of a pedantic category (though that was a while ago).

---

_Comment by @DetachHead on 2023-11-03 12:31_

Closing as a duplicate of #5182

---

_Closed by @DetachHead on 2023-11-03 12:31_

---
