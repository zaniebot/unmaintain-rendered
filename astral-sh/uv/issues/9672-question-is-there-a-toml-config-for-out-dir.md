```yaml
number: 9672
title: "Question: Is there a toml config for out-dir option of build command"
type: issue
state: open
author: dmurat
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2024-12-06T10:05:56Z
updated_at: 2024-12-26T15:59:06Z
url: https://github.com/astral-sh/uv/issues/9672
synced_at: 2026-01-12T15:59:56Z
```

# Question: Is there a toml config for out-dir option of build command

---

_@dmurat_

I can adjust the output directory of the build with the `--out-dir` CLI option. However, I would like to specify this in `pyproject.toml`. Is there a configuration option to do this?

Thanks!

---

_Comment by @charliermarsh on 2024-12-06 11:53_

There isn't one today. I don't know if it would make sense to add one since it's coupled to a specific uv subcommand. Curious to hear others' opinions though.

---

_Label `question` added by @charliermarsh on 2024-12-06 11:53_

---

_Label `question` removed by @charliermarsh on 2024-12-26 15:59_

---

_Label `configuration` added by @charliermarsh on 2024-12-26 15:59_

---

_Label `needs-decision` added by @charliermarsh on 2024-12-26 15:59_

---
