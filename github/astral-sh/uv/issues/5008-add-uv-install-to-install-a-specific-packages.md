---
number: 5008
title: "Add `uv install` to install a specific packages from the workspace"
type: issue
state: closed
author: bschoenmaeckers
labels:
  - cli
  - preview
assignees: []
created_at: 2024-07-12T13:48:46Z
updated_at: 2024-07-31T15:27:19Z
url: https://github.com/astral-sh/uv/issues/5008
synced_at: 2026-01-07T13:12:17-06:00
---

# Add `uv install` to install a specific packages from the workspace

---

_Issue opened by @bschoenmaeckers on 2024-07-12 13:48_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I basically  want to install a subset of packages from my virtual workspace in a Dockerfile with respect to the uv.lock file.
Currently I use `uv sync` to install all locked packages including the whole workspace. But I don't see a way to install just one packages (including its dependencies).

I tried to do `uv pip install -c uv.lock packages/package_a` but this obviously does not work because uv.lock is not a constrain file.



---

_Referenced in [astral-sh/uv#5653](../../astral-sh/uv/issues/5653.md) on 2024-07-31 14:08_

---

_Comment by @zanieb on 2024-07-31 14:21_

I thought we had `uv sync --package <name>` but I guess not? cc @konstin 

---

_Comment by @charliermarsh on 2024-07-31 14:21_

Oh me too. I will add it.

---

_Label `cli` added by @charliermarsh on 2024-07-31 14:21_

---

_Label `preview` added by @charliermarsh on 2024-07-31 14:21_

---

_Assigned to @charliermarsh by @konstin on 2024-07-31 14:21_

---

_Comment by @charliermarsh on 2024-07-31 14:24_

To be clear, this would be intended to support installing a specific _member_ (like `package/package_a` above), rather than installing a specific transitive dependency (like `flask` or whatever).

---

_Referenced in [astral-sh/uv#5656](../../astral-sh/uv/pulls/5656.md) on 2024-07-31 14:54_

---

_Closed by @charliermarsh on 2024-07-31 15:16_

---

_Comment by @bschoenmaeckers on 2024-07-31 15:27_

Thanks a lot! This will do for my usecase.

---
