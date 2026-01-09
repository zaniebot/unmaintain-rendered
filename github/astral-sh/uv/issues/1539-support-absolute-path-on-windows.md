---
number: 1539
title: Support absolute path on windows
type: issue
state: closed
author: Czaki
labels:
  - bug
  - windows
assignees: []
created_at: 2024-02-16T20:43:54Z
updated_at: 2024-02-20T01:36:55Z
url: https://github.com/astral-sh/uv/issues/1539
synced_at: 2026-01-07T13:12:16-06:00
---

# Support absolute path on windows

---

_Issue opened by @Czaki on 2024-02-16 20:43_

When plaing with tox integration I got two bug reports that suggest that uv is not properly interpreting absolute windows paths. 

When try locally 
```
uv pip install napari@C:\Users\Grzegorz\Documents\Projects\napari\dist\napari-0.5.0a2.dev542+g9ad565f7.tar.gz
⠙ Resolving dependencies...
thread 'main' panicked at D:\a\uv\uv\crates\uv-distribution\src\source\mod.rs:168:22:
Distribution must have a filename: UrlFilename(Url { scheme: "c", cannot_be_a_base: true, username: "", password: None, host: None, port: None, path: "\\Users\\Grzegorz\\Documents\\Projects\\napari\\dist\\napari-0.5.0a2.dev542+g9ad565f7.tar.gz", query: None, fragment: None })
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

On CI: https://github.com/napari/napari/actions/runs/7935590920/job/21669475977?pr=6668#step:12:51
```
py311-windows-pyqt6-no_cov: install_package> python -I -m uv pip install --force-reinstall --no-deps "napari @ D:\a\napari\napari\dist\napari-0.5.0a2.dev560+g2c217efd-py3-none-any.whl"
error: Unable to extract filename from URL: d:\a\napari\napari\dist\napari-0.5.0a2.dev560+g2c217efd-py3-none-any.whl
```

It looks like there is a problem with parsing the Windows path. Did I'm correct?

---

_Comment by @charliermarsh on 2024-02-16 20:45_

Yeah that seems wrong...

---

_Label `bug` added by @charliermarsh on 2024-02-16 20:45_

---

_Renamed from "Support absolute path on windos" to "Support absolute path on windows" by @Czaki on 2024-02-16 20:55_

---

_Label `windows` added by @MichaReiser on 2024-02-16 20:55_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-16 20:58_

---

_Comment by @charliermarsh on 2024-02-16 22:42_

Reproduced, will fix.

---

_Comment by @charliermarsh on 2024-02-17 01:37_

This didn't go out in v0.1.3, but will be fixed soon.

---

_Referenced in [astral-sh/uv#1676](../../astral-sh/uv/issues/1676.md) on 2024-02-19 01:33_

---

_Comment by @gaborbernat on 2024-02-19 01:33_

This is blocking currently `tox-uv` supporting Windows (https://github.com/tox-dev/tox-uv/pull/19).

---

_Referenced in [tox-dev/tox-uv#19](../../tox-dev/tox-uv/pulls/19.md) on 2024-02-19 01:34_

---

_Comment by @charliermarsh on 2024-02-19 01:56_

👍 I’m hoping to fix it tomorrow.

---

_Referenced in [astral-sh/uv#1725](../../astral-sh/uv/pulls/1725.md) on 2024-02-20 01:03_

---

_Closed by @charliermarsh on 2024-02-20 01:36_

---

_Referenced in [LiberTEM/LiberTEM#1598](../../LiberTEM/LiberTEM/pulls/1598.md) on 2024-02-20 08:06_

---
