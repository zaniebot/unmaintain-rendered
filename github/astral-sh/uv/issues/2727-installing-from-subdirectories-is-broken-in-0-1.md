---
number: 2727
title: Installing from subdirectories is broken in 0.1.26 (and 0.1.25 probably)
type: issue
state: closed
author: abdulhaq-e
labels:
  - bug
assignees: []
created_at: 2024-03-30T01:07:52Z
updated_at: 2024-03-30T02:37:11Z
url: https://github.com/astral-sh/uv/issues/2727
synced_at: 2026-01-07T13:12:17-06:00
---

# Installing from subdirectories is broken in 0.1.26 (and 0.1.25 probably)

---

_Issue opened by @abdulhaq-e on 2024-03-30 01:07_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

A  dependency defined using a subdirectory fails to generate the lockfile in `0.1.26`. 

An example dependency is: `"mersal @ git+ssh://git@github.com/mersal-org/mersal.git@trunk#subdirectory=core"`

The error message is:

```
error: Failed to download and build: mersal @ git+ssh://git@github.com/mersal-org/mersal.git@trunk#subdirectory=core
  Caused by: Package metadata name `mersal-virtual-workspace` does not match given name `mersal`
error: could not write production lockfile for project
```

The error was originally obtained using `rye sync` but then I switched to using `uv` directly via `uv pip compile pyproject.toml -o requirements.txt`

What is happening: The process is finding a `pyproject.toml` at the root directory which has a package metadata name `mersal-virtual-workspace`. This didn't happen in version `0.1.24` or before (I tested from `01.20`).

With `0.1.25`, I get a different error which I believe is now resolved in `0.1.26` (https://github.com/astral-sh/uv/pull/2712):

```
 Caused by: Failed to deserialize cache entry
  Caused by: expected version to start with a number, but no leading ASCII digits were found
```

However, looking at the changelog, the original error was probably caused by a change in `0.1.25`. 




---

_Comment by @abdulhaq-e on 2024-03-30 01:10_

If I had to guess, it's probably not the subdirectory handling itself being broken but the existence of a `pyproject.toml` in the root directory is throwing the resolver.

---

_Renamed from "Subdirectories are broken in 0.1.26 (and 0.1.25 probably)" to "Installing from subdirectories is broken in 0.1.26 (and 0.1.25 probably)" by @abdulhaq-e on 2024-03-30 01:16_

---

_Comment by @zanieb on 2024-03-30 01:30_

Thanks for the thorough report! If you have time, could you share a reproduction with a public repository?

Note for us, we should add test coverage for this in a new [`astral-test`](https://github.com/orgs/astral-test/repositories) repository (e.g. `uv-pypackage-workspace`).

cc @charliermarsh

---

_Label `bug` added by @zanieb on 2024-03-30 01:30_

---

_Comment by @charliermarsh on 2024-03-30 01:35_

Not immediately obvious to me how this would be happening.

---

_Comment by @charliermarsh on 2024-03-30 01:36_

Sorry, yes it is. Easy fix, my mistake.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-30 01:36_

---

_Referenced in [astral-sh/uv#2728](../../astral-sh/uv/pulls/2728.md) on 2024-03-30 02:15_

---

_Closed by @charliermarsh on 2024-03-30 02:36_

---

_Comment by @charliermarsh on 2024-03-30 02:37_

Fixed in the next release, apologies.

---
