---
number: 7115
title: "`uv sync --python-preference=only-system` fallback beyond `.python-version`"
type: issue
state: closed
author: jamesbraza
labels:
  - question
assignees: []
created_at: 2024-09-06T04:48:18Z
updated_at: 2024-09-06T17:40:50Z
url: https://github.com/astral-sh/uv/issues/7115
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv sync --python-preference=only-system` fallback beyond `.python-version`

---

_Issue opened by @jamesbraza on 2024-09-06 04:48_

With `uv==0.4.6`, running `uv sync --python-preference=only-system` in GitHub Actions runner `ubuntu-24.04`:

- Without `.python-version` file: works fine
- With `.python-version` file containing `3.12.5`: `error: No interpreter found for Python 3.12.5 in system path`

This is because `ubuntu-24.04` has Python 3.12.3 (and not 3.12.5) at `/usr/bin/python3`.

I guess my question is (and also a possible feature request), can `uv sync --python-preference=only-system` have a way to fall back beyond `.python-version`, if the OS contains a Python version that doesn't match the version detailed by `.python-version` file?

---

_Comment by @zanieb on 2024-09-06 13:24_

It sounds like you should just have `3.12` in your `.python-version` file? I think this behavior is correct, in that you've asked for a specific patch version and we should fail if we cannot fulfill that request. You can use `--no-config` to ignore the `.python-version` file if necessary.

---

_Label `question` added by @zanieb on 2024-09-06 13:24_

---

_Comment by @jamesbraza on 2024-09-06 17:40_

Oh wow I actually didn't know you can have just a major/minor version in `.python-version`. TIL!

Thanks again and sorry for a false positive request

---

_Closed by @jamesbraza on 2024-09-06 17:40_

---
