---
number: 17181
title: "`uv pip install --no-binary` does not accept `,` as delimiter"
type: issue
state: closed
author: shumpohl
labels:
  - bug
  - help wanted
  - compatibility
assignees: []
created_at: 2025-12-18T19:16:49Z
updated_at: 2025-12-19T13:58:36Z
url: https://github.com/astral-sh/uv/issues/17181
synced_at: 2026-01-10T01:26:14Z
---

# `uv pip install --no-binary` does not accept `,` as delimiter

---

_Issue opened by @shumpohl on 2025-12-18 19:16_

### Summary

## pip 25.1.1
```bash
pip install --no-binary=numpy,scipy numpy scipy
```

works as officially documented: https://pip.pypa.io/en/stable/cli/pip_install/#cmdoption-no-binary

## uv 0.9.18

```bash
$ uv pip install --no-binary=numpy,scipy numpy scipy
error: invalid value 'numpy,scipy' for '--no-binary <NO_BINARY>': Not a valid package or extra name: "numpy,scipy". Names must start and end with a letter or digit and may only contain -, _, ., and alphanumeric characters.

For more information, try '--help'.
```

~~However, `--no-binary=numpy;scipy` works with uv.~~ (this was my shell)

### Platform

Linux 6.8.0-90-generic x86_64 GNU/Linux

### Version

uv 0.9.18

### Python version

_No response_

---

_Label `bug` added by @shumpohl on 2025-12-18 19:16_

---

_Renamed from "`uv pip install --no-binary` uses `;` instead of `,` as delimiter" to "`uv pip install --no-binary` does not accept `,` as delimiter" by @shumpohl on 2025-12-18 19:27_

---

_Label `compatibility` added by @zanieb on 2025-12-18 19:46_

---

_Comment by @zanieb on 2025-12-18 19:46_

Seems reasonable to support this. We already have various `CommaSeparated...` types.

---

_Label `help wanted` added by @zanieb on 2025-12-18 19:47_

---

_Referenced in [astral-sh/uv#17185](../../astral-sh/uv/pulls/17185.md) on 2025-12-19 00:14_

---

_Closed by @zanieb on 2025-12-19 13:58_

---
