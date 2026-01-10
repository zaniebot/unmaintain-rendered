---
number: 11725
title: specify virtual env with uv run
type: issue
state: closed
author: insane-dreamer
labels:
  - question
assignees: []
created_at: 2025-02-23T16:04:24Z
updated_at: 2025-02-24T17:45:43Z
url: https://github.com/astral-sh/uv/issues/11725
synced_at: 2026-01-10T01:25:09Z
---

# specify virtual env with uv run

---

_Issue opened by @insane-dreamer on 2025-02-23 16:04_

### Summary

It would be great if one could specify the venv to use when invoking uv run. I know it's possible to set an environment variable for the project environment, but such an argument would be a more convenient way to run an app with a different environment than the default project environment (in .venv). Often it's necessary to test code with different sets of specific package versions (not just a different python). 

Something like `uv run --env <venv name or folder> app.py`

This would be the equivalent of doing `conda activate <env>; python app.py`



### Example

_No response_

---

_Label `enhancement` added by @insane-dreamer on 2025-02-23 16:04_

---

_Comment by @konstin on 2025-02-24 15:01_

You can select a specific venv with the `--python` option, such as `uv run --python myvenv/bin/python`.

---

_Label `enhancement` removed by @konstin on 2025-02-24 15:02_

---

_Label `question` added by @konstin on 2025-02-24 15:02_

---

_Closed by @insane-dreamer on 2025-02-24 17:45_

---
