---
number: 16065
title: "Enable overriding `pyproject.toml` python version (and `.python-version`) with `uv python pin`"
type: issue
state: closed
author: Parskatt
labels:
  - enhancement
assignees: []
created_at: 2025-09-29T13:03:49Z
updated_at: 2025-09-30T05:04:24Z
url: https://github.com/astral-sh/uv/issues/16065
synced_at: 2026-01-07T13:12:19-06:00
---

# Enable overriding `pyproject.toml` python version (and `.python-version`) with `uv python pin`

---

_Issue opened by @Parskatt on 2025-09-29 13:03_

### Summary

Let's say you have some project with python version X.
Likely your `pyproject.toml` says `requires-python = ">=X"` and your `.python-version` says `X`
Now you want to add a dependency D.
D is not available for X, but is available for Y < X.

You now need to adjust your python version.
Only way I'm aware of how to do this is to manually edit `pyproject.toml` and `.python-version` to Y.
This is not a big deal, but gets annoying.

### Example

One way to fix this could be to enable an override flag in `uv python pin Y --override-env` to force the env to conform to the pinned python version.

If there is another existing way it might be good to update https://docs.astral.sh/uv/reference/cli/#uv-python , couldn't find anything there.

---

_Label `enhancement` added by @Parskatt on 2025-09-29 13:03_

---

_Closed by @Parskatt on 2025-09-30 05:04_

---
