---
number: 645
title: "Add \"severity\" to check and message API"
type: issue
state: closed
author: charliermarsh
labels:
  - core
assignees: []
created_at: 2022-11-07T19:18:47Z
updated_at: 2022-12-31T18:27:58Z
url: https://github.com/astral-sh/ruff/issues/645
synced_at: 2026-01-07T13:12:14-06:00
---

# Add "severity" to check and message API

---

_Issue opened by @charliermarsh on 2022-11-07 19:18_

This is part of the LSP diagnostic API so we should implement it.

---

_Comment by @jhossbach on 2022-11-07 21:45_

Out of curiosity, do you plan to mark every violation that results in a error to mark as 'error' according to the LSP API? Especially for flake8, there are a few `F...` codes that don't strictly result in an error in your code and should be declared as a Warning, such as e.g. `F401`, but then there are also codes like e.g. `F523` that would definitely result in an error. 

---

_Comment by @charliermarsh on 2022-11-08 21:16_

I think we should probably differentiate between errors and warnings, though I need to do some research on how linters typically differentiate between the two. (In ESLint, you can also declare each code to be an error, or a warning, or whatever.) Definitely open to feedback here on what a good system would look like!

---

_Comment by @jhossbach on 2022-11-13 14:40_

I tried finding the severity rules for flake8 in the python extension for vscode (https://github.com/microsoft/vscode-python) but I was unsuccessful. I would say that everything resulting in a error in python should be marked as "error" (obviously), everything else should be considered a warning. I am not sure when Information is useful, and hint maybe wherever a fix is possible?

Maybe you have more luck deciphering the TS code, I never had any issues with the severity flags in vscode so they might be a good inspiration.

---

_Label `core` added by @charliermarsh on 2022-12-31 18:19_

---

_Comment by @charliermarsh on 2022-12-31 18:27_

Closing in favor of #1256.

---

_Closed by @charliermarsh on 2022-12-31 18:27_

---
