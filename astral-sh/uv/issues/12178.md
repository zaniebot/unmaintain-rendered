```yaml
number: 12178
title: uv python pin --global fails when uv directory does not exists
type: issue
state: closed
author: samypr100
labels:
  - bug
assignees: []
created_at: 2025-03-15T00:45:02Z
updated_at: 2025-03-15T02:50:47Z
url: https://github.com/astral-sh/uv/issues/12178
synced_at: 2026-01-10T03:50:31Z
```

# uv python pin --global fails when uv directory does not exists

---

_Issue opened by @samypr100 on 2025-03-15 00:45_

### Summary

When running `uv python pin --global` introduced in https://github.com/astral-sh/uv/pull/12115, it can fail if the `uv` directory does not exists already.

Linux

```console
$ uv python pin --global 3.13
error: failed to write to file `$HOME/.config/uv/.python-version`: No such file or directory (os error 2)
```

Windows

```console
> uv python pin --global 3.13
error: failed to write to file `%APPDATA%\Roaming\uv\.python-version`: The system cannot find the path specified. (os error 3)
```

### Platform

Windows, Ubuntu

### Version

0.6.7 debug build

### Python version

3.13.2

---

_Label `bug` added by @samypr100 on 2025-03-15 00:45_

---

_Comment by @charliermarsh on 2025-03-15 00:46_

Nice, thanks. (Do you wanna take a look at this?)

---

_Closed by @charliermarsh on 2025-03-15 02:50_

---

_Closed by @charliermarsh on 2025-03-15 02:50_

---
