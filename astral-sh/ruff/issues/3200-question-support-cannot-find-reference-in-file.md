```yaml
number: 3200
title: "Question: Support \"Cannot find reference in file\" (incorrect import)"
type: issue
state: closed
author: bbkgh
labels: []
assignees: []
created_at: 2023-02-24T04:00:02Z
updated_at: 2023-02-24T04:42:15Z
url: https://github.com/astral-sh/ruff/issues/3200
synced_at: 2026-01-12T15:54:43Z
```

# Question: Support "Cannot find reference in file" (incorrect import)

---

_@bbkgh_

Hi. Thanks for this great project. I have a question about finding incorrect/non-existing class/methods imports in python with static analysers. Is there any static analyser here that can find this type of errors? (probably with some false-positives)
```
from models.X import Y   # Y doesn't exists in X
print(Y)
```
PyCharm's built-in analyser hints this with "Cannot find reference ' X' in 'Y' ".
Is this somehow supported in ruff or any other tools?

---

_Comment by @charliermarsh on 2023-02-24 04:01_

I'd like Ruff to be able to do this but it can't do it yet, and I've prioritized some other things above that kind of analysis. So it's within-scope for the project, but there's not a clear timeline on when it would happen.

(Your best bet, if you want to perform that kind of analysis, is to use a type checker, like [Pyright](https://github.com/microsoft/pyright) or [mypy](https://github.com/python/mypy).)

---

_Comment by @charliermarsh on 2023-02-24 04:42_

Closing for now since it's a little large for an issue.

---

_Closed by @charliermarsh on 2023-02-24 04:42_

---
