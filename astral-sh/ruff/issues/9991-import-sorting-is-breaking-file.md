```yaml
number: 9991
title: Import sorting is breaking file
type: issue
state: closed
author: theelderbeever
labels:
  - bug
  - isort
assignees: []
created_at: 2024-02-14T21:48:50Z
updated_at: 2024-02-16T16:52:59Z
url: https://github.com/astral-sh/ruff/issues/9991
synced_at: 2026-01-12T15:54:49Z
```

# Import sorting is breaking file

---

_@theelderbeever_

I am using the vscode sort on save setting and the following happens when I save the file. Variations of this happen on multiple files to the point of affecting code as well.

Before:
<img width="711" alt="image" src="https://github.com/astral-sh/ruff/assets/29295582/47c0ae11-8741-4442-8fcc-eaa29a8d4e73">

After:
<img width="654" alt="image" src="https://github.com/astral-sh/ruff/assets/29295582/f747520d-3893-49d6-9c84-9bfdae2a69ad">


---

_Comment by @theelderbeever on 2024-02-14 21:50_

I think it has something to do with trailing commas in tuples

---

_Label `bug` added by @zanieb on 2024-02-14 22:02_

---

_Label `isort` added by @zanieb on 2024-02-14 22:02_

---

_Comment by @zanieb on 2024-02-14 22:02_

Thanks for the report!

If you have time could you provide a minimal example (as text rather than a screenshot) that reproduces the problem? It'd be helpful to narrow down what exactly causes the problem.

---

_Comment by @charliermarsh on 2024-02-15 00:21_

Do you have the isort extension enabled? To me, this looks a lot like https://github.com/astral-sh/ruff-vscode/issues/128 which unfortunately is actually an unfixed VS Code bug (https://github.com/microsoft/vscode/issues/174295) that results from having two extensions that both attempt to modify imports. Can you try disabling the isort extension?

---

_Comment by @theelderbeever on 2024-02-16 16:52_

@charliermarsh I had wondered if `isort` was implicated in this but, forgot that there was a dedicated extension for it. Just turned it off. The issue is weirdly intermittent so I won't know if it fixes the problem for a bit. I think we can close for now and I can just reopen with better examples for @zanieb if it recurs.

---

_Closed by @theelderbeever on 2024-02-16 16:52_

---
