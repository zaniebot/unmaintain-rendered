```yaml
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
synced_at: 2026-01-12T15:54:53Z
```

# [flake8-todos] TD rules trigger on words beginning with todo

---

_@autinerd_

Hi,

currently the TD rules trigger on words beginning with "todo", like `# Todoist API`

I think it should only trigger on the whole word "todo"

Thanks!

---

_Comment by @zanieb on 2024-10-05 15:13_

What's the upstream plugin do?

---

_Comment by @autinerd on 2024-10-05 19:16_

I looked in the code of that, and yeah, it does the same as in using the regex `#\s*({tags})` and not looking if it is part of a word.

---

_Closed by @MichaReiser on 2024-10-14 10:21_

---
