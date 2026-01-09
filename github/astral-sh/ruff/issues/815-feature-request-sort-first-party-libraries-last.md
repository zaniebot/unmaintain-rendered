---
number: 815
title: "Feature request: sort first-party libraries last"
type: issue
state: closed
author: tekumara
labels: []
assignees: []
created_at: 2022-11-19T08:34:44Z
updated_at: 2022-11-20T00:16:27Z
url: https://github.com/astral-sh/ruff/issues/815
synced_at: 2026-01-07T13:12:14-06:00
---

# Feature request: sort first-party libraries last

---

_Issue opened by @tekumara on 2022-11-19 08:34_

Ruff currently sorts imports without distinction between first-party and third-party libraries.

For feature parity with isort, it would be great if ruff could sort first-party libraries into their own section at the end of the imports. 😍 


---

_Comment by @charliermarsh on 2022-11-19 14:28_

This should be the current behavior! Can you try setting the `src` path in your `pyproject.toml`, like:

```toml
[tool.ruff]
src = [".", "tools", "tools/setup/emoji"]
```

---

_Comment by @tekumara on 2022-11-20 00:03_

Oh fantastic that works, thank you!

---

_Closed by @tekumara on 2022-11-20 00:03_

---

_Comment by @charliermarsh on 2022-11-20 00:16_

Sweet!

---
