---
number: 13638
title: "[flake8-todos] TD rules trigger on words beginning with todo"
type: issue
state: closed
author: autinerd
labels: []
assignees: []
created_at: 2024-10-05T08:28:13Z
updated_at: 2024-10-14T10:21:47Z
url: https://github.com/astral-sh/ruff/issues/13638
synced_at: 2026-01-07T13:12:15-06:00
---

# [flake8-todos] TD rules trigger on words beginning with todo

---

_Issue opened by @autinerd on 2024-10-05 08:28_

Hi,

currently the TD rules trigger on words beginning with "todo", like `# Todoist API`

I think it should only trigger on the whole word "todo"

Thanks!

---

_Referenced in [astral-sh/ruff#13640](../../astral-sh/ruff/pulls/13640.md) on 2024-10-05 09:51_

---

_Comment by @zanieb on 2024-10-05 15:13_

What's the upstream plugin do?

---

_Comment by @autinerd on 2024-10-05 19:16_

I looked in the code of that, and yeah, it does the same as in using the regex `#\s*({tags})` and not looking if it is part of a word.

---

_Closed by @MichaReiser on 2024-10-14 10:21_

---
