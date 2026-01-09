---
number: 2457
title: "Question: How can I ignore a rule in one directory?"
type: issue
state: closed
author: MartinThoma
labels: []
assignees: []
created_at: 2023-02-01T22:11:35Z
updated_at: 2023-02-01T23:10:15Z
url: https://github.com/astral-sh/ruff/issues/2457
synced_at: 2026-01-07T13:12:14-06:00
---

# Question: How can I ignore a rule in one directory?

---

_Issue opened by @MartinThoma on 2023-02-01 22:11_

Thanks for this awesome tool :pray: 

I would like to enable all rules, but disable some in some directories. For example `S101 Use of assert detected` should be disabled in the `tests` directory (but only there). How do I do that?

---

_Comment by @sbrugman on 2023-02-01 22:25_

This can be achieved using the following settings in `pyproject.toml`:

```
[tool.ruff.per-file-ignores]
"tests/*" = ["S101"]
```

---

_Comment by @MartinThoma on 2023-02-01 23:06_

Nice! Thank you ❤️

---

_Closed by @MartinThoma on 2023-02-01 23:06_

---

_Comment by @charliermarsh on 2023-02-01 23:10_

If you're having any trouble debugging those paths, `ruff --show-settings /path/to/file.py` will show you the resolved configuration including the regular expressions, and `ruff -v /path/to/file.py` will include debug logging that will tell you when a file is matched, and to what codes.

---
