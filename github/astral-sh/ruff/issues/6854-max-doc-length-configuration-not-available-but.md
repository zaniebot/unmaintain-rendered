---
number: 6854
title: max-doc-length configuration not available but documented
type: issue
state: closed
author: amrit110
labels:
  - question
  - needs-info
assignees: []
created_at: 2023-08-24T16:58:24Z
updated_at: 2023-08-26T14:37:10Z
url: https://github.com/astral-sh/ruff/issues/6854
synced_at: 2026-01-07T13:12:15-06:00
---

# max-doc-length configuration not available but documented

---

_Issue opened by @amrit110 on 2023-08-24 16:58_

Hi, it seems like the [max-doc-length](https://beta.ruff.rs/docs/settings/#pycodestyle-max-doc-length) option is not available as of v0.0.285. I get the error:

```bash
unknown field `max-doc-length`, expected one of `convention`, `ignore-decorators`, `property-decorators`
```


---

_Comment by @charliermarsh on 2023-08-24 18:16_

It looks like you're providing that under `tool.ruff.pydocstyle` based on the message. Can you try this?

```toml
[tool.ruff.pycodestyle]
max-doc-length = 88
```

---

_Label `waiting-on-author` added by @charliermarsh on 2023-08-24 18:16_

---

_Label `question` added by @zanieb on 2023-08-24 19:59_

---

_Comment by @amrit110 on 2023-08-26 14:36_

Oh my bad, thanks this works!

---

_Closed by @amrit110 on 2023-08-26 14:36_

---
