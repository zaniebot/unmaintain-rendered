---
number: 2275
title: Prefer Single Quotes
type: issue
state: closed
author: jackdesert
labels: []
assignees: []
created_at: 2023-01-27T20:41:32Z
updated_at: 2023-01-27T21:51:48Z
url: https://github.com/astral-sh/ruff/issues/2275
synced_at: 2026-01-07T13:12:14-06:00
---

# Prefer Single Quotes

---

_Issue opened by @jackdesert on 2023-01-27 20:41_

How do I get this message to prefer single quotes instead of double?

Version: ruff 0.0.236

    # run.py
    greeting = 'Hello'

`$ ruff --select Q --isolated run.py`

Message: 
`run.py:1:12: Q000 Single quotes found but double quotes preferred`

But the README says that the message for Q000 is "Double quotes found but single quotes preferred".
(Screenshot below)
<img width="1004" alt="image" src="https://user-images.githubusercontent.com/791315/215192390-978e45dd-e607-4896-8a39-8d20f2d3b33f.png">



---

_Comment by @charliermarsh on 2023-01-27 20:56_

It’s configurable via pyproject.toml. The default is to prefer double quotes: https://github.com/charliermarsh/ruff#flake8-quotes

(I can paste a code snippet when I’m back at my computer, but hopefully the docs give you a good lead!)

(I should probably change the docs to reflect the default.)

---

_Comment by @jackdesert on 2023-01-27 21:51_

I was actually delighted to see the version the README---it gave me hope that preferring single quotes must be an option somehow :)

---

_Comment by @jackdesert on 2023-01-27 21:51_

By the way I did get pyproject.toml connected and was able to configure flake8-quotes there :)

---

_Closed by @jackdesert on 2023-01-27 21:51_

---
