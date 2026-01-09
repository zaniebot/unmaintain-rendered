---
number: 6327
title: "uv python install doesn't put artifacts in configured cache dir"
type: issue
state: closed
author: MattiasDC
labels:
  - documentation
  - cache
assignees: []
created_at: 2024-08-21T13:55:54Z
updated_at: 2024-08-22T01:01:15Z
url: https://github.com/astral-sh/uv/issues/6327
synced_at: 2026-01-07T13:12:17-06:00
---

# uv python install doesn't put artifacts in configured cache dir

---

_Issue opened by @MattiasDC on 2024-08-21 13:55_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

We have the environment variable `UV_CACHE_DIR` in combination with cache sharing across users. It seems that when a python version is downloaded, it doesn't end up in the cache dir. Resulting in each user downloading the python version separately.

I'm assuming this isn't intended behavior as `uv python install --help` does include the 'Cache options' section


---

_Renamed from "uv python install put artifacts in configured cache dir" to "uv python install puts artifacts in configured cache dir" by @MattiasDC on 2024-08-21 13:56_

---

_Comment by @charliermarsh on 2024-08-21 14:01_

Yeah the Python versions are installed into the "data" directory rather than the "cache" directory. You can set `UV_PYTHON_INSTALL_DIR` or `XDG_DATA_HOME` to control this.

---

_Label `documentation` added by @charliermarsh on 2024-08-21 14:01_

---

_Label `cache` added by @charliermarsh on 2024-08-21 14:01_

---

_Renamed from "uv python install puts artifacts in configured cache dir" to "uv python install doesn't put artifacts in configured cache dir" by @MattiasDC on 2024-08-21 14:05_

---

_Comment by @baggiponte on 2024-08-21 14:32_

You can also see the dir running `uv python dir` :)

---

_Comment by @charliermarsh on 2024-08-22 01:01_

It's documented here: https://docs.astral.sh/uv/configuration/environment/

---

_Closed by @charliermarsh on 2024-08-22 01:01_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-22 01:01_

---
