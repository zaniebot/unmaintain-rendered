---
number: 13188
title: "Inconsistent option spelling between `uv pip` and `pip`"
type: issue
state: closed
author: calebho
labels:
  - question
assignees: []
created_at: 2025-04-29T01:25:45Z
updated_at: 2025-04-29T18:25:13Z
url: https://github.com/astral-sh/uv/issues/13188
synced_at: 2026-01-07T13:12:18-06:00
---

# Inconsistent option spelling between `uv pip` and `pip`

---

_Issue opened by @calebho on 2025-04-29 01:25_

### Summary

`pip` uses `--config-settings` (plural) whereas `uv pip` uses `--config-setting` (singular)
```
$ uv pip install --help | grep config-setting
  -C, --config-setting <CONFIG_SETTING>

$ uv run python -m pip install --help | grep config-setting
  -C, --config-settings <settings>
                              KEY=VALUE. Use multiple --config-settings
```


### Platform

Linux 6.1.131-143.221.amzn2023.x86_64 x86_64 GNU/Linux

### Version

uv 0.6.17 (8414e9f3d 2025-04-25)

### Python version

Python 3.10.12

---

_Label `bug` added by @calebho on 2025-04-29 01:25_

---

_Comment by @charliermarsh on 2025-04-29 01:43_

We accept both `--config-setting` and `--config-settings` -- they're aliases.

---

_Label `bug` removed by @charliermarsh on 2025-04-29 01:43_

---

_Label `question` added by @charliermarsh on 2025-04-29 01:43_

---

_Comment by @zanieb on 2025-04-29 03:50_

You can see these in the reference docs, e.g., at https://docs.astral.sh/uv/reference/cli/#uv-pip-compile--config-setting

They're omitted from the CLI help menu because we only have so much space.

---

_Comment by @calebho on 2025-04-29 18:25_

Ah makes sense thanks all

---

_Closed by @calebho on 2025-04-29 18:25_

---
