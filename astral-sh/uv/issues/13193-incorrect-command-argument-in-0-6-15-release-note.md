```yaml
number: 13193
title: Incorrect command argument in 0.6.15 Release Note
type: issue
state: closed
author: Maxioum
labels:
  - documentation
assignees: []
created_at: 2025-04-29T13:25:14Z
updated_at: 2025-04-29T13:33:50Z
url: https://github.com/astral-sh/uv/issues/13193
synced_at: 2026-01-12T16:01:21Z
```

# Incorrect command argument in 0.6.15 Release Note

---

_@Maxioum_

### Summary

In the [0.6.15](https://github.com/astral-sh/uv/releases/tag/0.6.15) release notes it's written:

- To generate a pylock.toml file from a set of requirements, `run: uv pip compile -o pylock.toml -r requirements.in`

When it should be:
`uv pip compile -o pylock.toml requirements.in` (no `-r`)

as per the [documentation](https://docs.astral.sh/uv/reference/cli/?query=pip+compile#uv-pip-compile)

Using the command with `-r` raises the error:

```bash

> uv pip compile -o pylock.toml -r requirements.in
error: unexpected argument '-r' found

 tip: to pass '-r' as a value, use '-- -r'

Usage: uv.exe pip compile [OPTIONS] <SRC_FILE|--group <GROUP>>

For more information, try '--help'.
```



### Platform

Windows 11

### Version

uv 0.6.14 (a4cec56dc 2025-04-09) / uv 0.6.15 (e2f400adb 2025-04-22) / uv 0.6.17 (8414e9f3d 2025-04-25)

### Python version

Python 3.11.10

---

_Label `bug` added by @Maxioum on 2025-04-29 13:25_

---

_Comment by @konstin on 2025-04-29 13:33_

Thanks!

---

_Label `bug` removed by @konstin on 2025-04-29 13:33_

---

_Label `documentation` added by @konstin on 2025-04-29 13:33_

---

_Closed by @konstin on 2025-04-29 13:33_

---
