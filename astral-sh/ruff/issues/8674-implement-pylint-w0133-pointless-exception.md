```yaml
number: 8674
title: implement pylint W0133 pointless-exception-statement
type: issue
state: closed
author: Yasu-umi
labels: []
assignees: []
created_at: 2023-11-14T10:44:13Z
updated_at: 2023-11-14T16:40:28Z
url: https://github.com/astral-sh/ruff/issues/8674
synced_at: 2026-01-12T15:54:48Z
```

# implement pylint W0133 pointless-exception-statement

---

_@Yasu-umi_

[pylint W0133 warning](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/pointless-exception-statement.html) is very useful feature that can help prevent human error.
```python
Exception  # bad
raise Exception  # good
raise Exception('some reason')  # good
```
If there doesn't seem to be any reason should not to implement it, I'm thinking of creating a pull request. What do you think?

---

_Comment by @charliermarsh on 2023-11-14 16:40_

I believe this is already covered by `useless-expression` (`B018`) -- can you give that a try? Here's an example: https://play.ruff.rs/624389a6-9342-4ff0-94e8-7cceb14c10c4

---

_Closed by @charliermarsh on 2023-11-14 16:40_

---
