---
number: 13375
title: "Support flags in requirements.in for `uv pip compile`"
type: issue
state: closed
author: howsiyu
labels:
  - enhancement
assignees: []
created_at: 2025-05-10T07:13:53Z
updated_at: 2025-05-10T14:29:42Z
url: https://github.com/astral-sh/uv/issues/13375
synced_at: 2026-01-10T01:25:32Z
---

# Support flags in requirements.in for `uv pip compile`

---

_Issue opened by @howsiyu on 2025-05-10 07:13_

### Summary

Running `uv pip compile requirements.in` with the file containing
```
-e . --config-settings editable_mode=strict
```
runs into
```
error: Expected `--hash`, found `"--config-settings"`
```

### Example

_No response_

---

_Label `enhancement` added by @howsiyu on 2025-05-10 07:13_

---

_Comment by @howsiyu on 2025-05-10 07:38_

Hmm I run into the same error when running `uv pip install -r requirements.txt` as well. I can run `uv pip install -r requirements.txt --config-settings editable_mode=strict` but then I can't customize for individual packages.

---

_Comment by @charliermarsh on 2025-05-10 14:29_

We don't support `--config-settings` in `requirements.txt` -- I think you're looking for https://github.com/astral-sh/uv/issues/3636

---

_Closed by @charliermarsh on 2025-05-10 14:29_

---
