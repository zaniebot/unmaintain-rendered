```yaml
number: 2242
title: Add --system/--python flag to uv pip compile
type: issue
state: closed
author: liiight
labels:
  - cli
assignees: []
created_at: 2024-03-06T15:38:25Z
updated_at: 2024-04-18T04:55:50Z
url: https://github.com/astral-sh/uv/issues/2242
synced_at: 2026-01-12T15:58:36Z
```

# Add --system/--python flag to uv pip compile

---

_@liiight_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Based on this: https://github.com/astral-sh/uv/issues/1526#issuecomment-1981135895

> I actually had a broken `.venv` dir which it auto discovered:
> ```bash
> $ uv pip compile be3_lib.txt
> error: Broken virtualenv `/repos/analytics_content/.venv`, it contains a pyvenv.cfg but no Python binary at > `/repos/analytics_content/.venv/bin/python`
> ```
> This is because I use this directory in a docker container which is mounted so it broke the venv symlink. After I removed the `.venv` dir it worked as expected.



---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-06 21:47_

---

_Label `cli` added by @charliermarsh on 2024-03-06 21:47_

---

_Closed by @charliermarsh on 2024-04-18 04:55_

---
