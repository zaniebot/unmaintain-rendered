```yaml
number: 8581
title: "Running `uv sync --locked` asked to update uv.lock even after `uv lock`"
type: issue
state: closed
author: ekzhu
labels:
  - bug
assignees: []
created_at: 2024-10-25T23:41:20Z
updated_at: 2024-10-27T09:02:57Z
url: https://github.com/astral-sh/uv/issues/8581
synced_at: 2026-01-12T15:59:29Z
```

# Running `uv sync --locked` asked to update uv.lock even after `uv lock`

---

_@ekzhu_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

To repreduce:

```
uv lock
uv sync --locked
```

```
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
```

uv 0.4.27

---

_Comment by @jackgerrits on 2024-10-25 23:42_

Issue should be reproducible on our workspace here:

https://github.com/microsoft/autogen/tree/main/python

---

_Assigned to @charliermarsh by @zanieb on 2024-10-26 16:23_

---

_Label `bug` added by @zanieb on 2024-10-26 16:24_

---

_Comment by @charliermarsh on 2024-10-26 16:28_

Thanks for the repro.

---

_Comment by @charliermarsh on 2024-10-26 17:21_

Apologies. Fixed in the next release. You can also workaround it by removing the empty `dev-dependencies` list here (we weren't handling the empty case correctly when validating): https://github.com/microsoft/autogen/blob/3fe0f9e97d67a67f4027a92f458cb4842b3db43f/python/packages/autogen-agentchat/pyproject.toml#L22.

---

_Closed by @charliermarsh on 2024-10-26 17:29_

---

_Comment by @jackgerrits on 2024-10-27 09:02_

Thanks so much for the quick fix and explanation!

---
