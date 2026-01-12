```yaml
number: 7995
title: "Request: porting `cibuildwheel` to `uv build`"
type: issue
state: open
author: jamesbraza
labels:
  - wish
assignees: []
created_at: 2024-10-08T05:33:56Z
updated_at: 2024-10-22T23:59:02Z
url: https://github.com/astral-sh/uv/issues/7995
synced_at: 2026-01-12T15:59:18Z
```

# Request: porting `cibuildwheel` to `uv build`

---

_@jamesbraza_

As of `uv==0.4.19`, it seems `uv build` supports `--wheel`, but it does not support platform-specific wheels like https://github.com/pypa/cibuildwheel.

The main benefit of `uv` porting this tool is enabling `uv` to be a one-stop-shop for everything Python package management, now also including making wheels for many different platforms and architectures.

---

https://github.com/astral-sh/uv/issues/1681 exists to port `pip wheel`, so I am making this separate request to port `cibuildwheel`.


---

_Comment by @paveldikov on 2024-10-10 13:21_

FWIW, `cibuildwheel` is quite a complex piece of machinery (requires container runtime, access to `manylinux` images, ...). Perhaps porting `auditwheel`/`delocate` capabilities is a nice intermediate stepping stone?

---

_Label `wish` added by @charliermarsh on 2024-10-22 23:59_

---
