```yaml
number: 8164
title: uv tool install does not respect index-url from pyproject.toml
type: issue
state: closed
author: benjs
labels:
  - enhancement
assignees: []
created_at: 2024-10-14T06:24:28Z
updated_at: 2025-05-22T22:33:41Z
url: https://github.com/astral-sh/uv/issues/8164
synced_at: 2026-01-12T15:59:21Z
```

# uv tool install does not respect index-url from pyproject.toml

---

_@benjs_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
UV tool does not look into the pyproject.toml for already specified indices.

pyproject.toml:
```toml
[tool.uv]
index-url = "https://<my-index-url.com>/pypi/pypi/simple"
```

command:
```bash
uv tool install ruff
```

uv --version:
```
uv 0.4.18
```

Specifying the index url with `--index-url` worked, but is annoying :)
Thanks for this great tool!

---

_Comment by @charliermarsh on 2024-10-14 14:15_

Thanks! This is actually intentional right now. We have a separate design for "project-level tools" (i.e., tools that would be versioned and installed as part of the project, but given their own independent virtual environments) that we want to implement to solve this specific use-case. It's part of #3560.

---

_Comment by @charliermarsh on 2024-10-14 14:16_

I'm gonna close in favor of that issue since we're tracking it centrally as part of that effort.

---

_Closed by @charliermarsh on 2024-10-14 14:16_

---

_Closed by @charliermarsh on 2024-10-14 14:16_

---

_Label `enhancement` added by @charliermarsh on 2024-10-14 14:16_

---

_Comment by @KyzEver on 2025-03-19 13:33_

I'm also trying to configure `uv tool install` to adhere to a configuration provided in the pyproject.toml and not rely on the additional `--default-index` flag, preferably.

This issue was closed in favor of #3560, but that issue has also been resolved & closed without any mention of tool installation index. I would like this to be a feature of uv, if possible.

Is it acceptable to reopen this issue?

---

_Comment by @codethief on 2025-05-22 22:33_

@KyzEver I'd suggest we rather rally behind one of the open tickets, see [my comment here](https://github.com/astral-sh/uv/issues/3560#issuecomment-2902752225).

---
