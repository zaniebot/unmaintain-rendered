---
number: 15971
title: "`uv pip` does not recognize `PIP_*` environment variables"
type: issue
state: closed
author: gandaro
labels:
  - bug
assignees: []
created_at: 2025-09-22T00:51:33Z
updated_at: 2025-09-22T01:02:11Z
url: https://github.com/astral-sh/uv/issues/15971
synced_at: 2026-01-07T13:12:19-06:00
---

# `uv pip` does not recognize `PIP_*` environment variables

---

_Issue opened by @gandaro on 2025-09-22 00:51_

### Summary

Some options to pip can be supplied as environment variables, e.g. `PIP_NO_CACHE_DIR`, which corresponds to the `--no-cache-dir` option in pip or to the `UV_NO_CACHE` environment variable in uv, and `PIP_NO_COLOR`, which corresponds to the `--no-color` option in pip and to the `--color never` option in uv.

These options are not recognized by `uv pip`.

Thus, in order for `uv pip` to be truly compatible, treatment for some environment variables needs to be added.

(Note that the amount of environment variables that pip recognizes is quite large. Apparently every CLI flag has at least one associated environment variable. You can even set `PIP_HELP` as a substitute for `--help`.)

### Platform

any

### Version

any

### Python version

_No response_

---

_Label `bug` added by @gandaro on 2025-09-22 00:51_

---

_Comment by @charliermarsh on 2025-09-22 00:52_

We generally don't read configuration files or environment variables that are specifically designed for other tools -- it leads to too much complexity and confusing behavior. We expose our own settings as environment variables which you can see here: https://docs.astral.sh/uv/reference/environment/

---

_Closed by @charliermarsh on 2025-09-22 00:52_

---

_Comment by @gandaro on 2025-09-22 00:56_

In that case you may want to add that stance to the `uv pip` command reference or other documentation, so that users of `uv pip` know what to expect.

---

_Comment by @gandaro on 2025-09-22 01:02_

Never mind, I previously didn't find the relevant docs:

> uv does not read configuration files or environment variables that are specific to pip, like pip.conf or PIP_INDEX_URL. ([source](https://docs.astral.sh/uv/pip/compatibility/#configuration-files-and-environment-variables))

---
