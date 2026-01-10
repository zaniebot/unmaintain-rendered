```yaml
number: 7141
title: "Fix typo `aaarch64->aarch64`"
type: pull_request
state: merged
author: janosh
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-typo-aaarch64-apple-darwin
created_at: 2024-09-06T22:11:51Z
updated_at: 2024-09-06T23:25:46Z
url: https://github.com/astral-sh/uv/pull/7141
synced_at: 2026-01-10T12:53:41Z
```

# Fix typo `aaarch64->aarch64`

---

_Pull request opened by @janosh on 2024-09-06 22:11_

copy pasted `--python-platform aaarch64-unknown-linux-gnu` [from the docs](https://docs.astral.sh/uv/reference/cli/#uv-pip-compile) and got

> error: invalid value 'aaarch64-unknown-linux-gnu' for '--python-platform <PYTHON_PLATFORM>'
>   [possible values: windows, linux, macos, x86_64-pc-windows-msvc, i686-pc-windows-msvc, x86_64-unknown-linux-gnu, aarch64-apple-darwin, x86_64-apple-darwin, aarch64-unknown-linux-gnu, aarch64-unknown-linux-musl, x86_64-unknown-linux-musl, x86_64-manylinux_2_17, x86_64-manylinux_2_28, x86_64-manylinux_2_31, aarch64-manylinux_2_17, aarch64-manylinux_2_28, aarch64-manylinux_2_31]
> 
>   tip: a similar value exists: 'aarch64-unknown-linux-gnu'

---

_@charliermarsh approved on 2024-09-06 23:17_

Thank you!

---

_Label `documentation` added by @charliermarsh on 2024-09-06 23:18_

---

_Merged by @charliermarsh on 2024-09-06 23:25_

---

_Closed by @charliermarsh on 2024-09-06 23:25_

---
