```yaml
number: 9381
title: "[Feature Request] Make it default to read from .env-files"
type: issue
state: closed
author: dest1n1s
labels:
  - question
assignees: []
created_at: 2024-11-23T10:24:16Z
updated_at: 2025-01-07T19:49:50Z
url: https://github.com/astral-sh/uv/issues/9381
synced_at: 2026-01-12T15:59:48Z
```

# [Feature Request] Make it default to read from .env-files

---

_@dest1n1s_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Currently `uv` requires extra explicit settings to specify an .env-files, either by passing `--env-file .env` or an environmental variable `UV_ENV_FILE`. Since it has become a consensus to read environment variables from .env-files, it seems natural to make this a default behavior if any .env-file is detected, while allowing explicit setting `--no-env-file` flag or `UV_NO_ENV_FILE` environmental variable to disable this default loading. Node.js interpreter commands, like `npm run` and `bun run`, do the same thing.

---

_Comment by @zanieb on 2024-11-26 00:03_

There was a great deal of controversy about this when we added it initially, please see the discussion leading up to https://github.com/astral-sh/uv/issues/1384#issuecomment-2455270920.

---

_Label `question` added by @charliermarsh on 2024-12-16 22:07_

---

_Closed by @zanieb on 2025-01-07 19:49_

---
