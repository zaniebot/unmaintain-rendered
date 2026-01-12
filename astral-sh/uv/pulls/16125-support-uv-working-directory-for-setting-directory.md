```yaml
number: 16125
title: "Support `UV_WORKING_DIRECTORY` for setting `--directory`"
type: pull_request
state: merged
author: shunsock
labels:
  - configuration
assignees: []
merged: true
base: main
head: directory_option_allow_envvar
created_at: 2025-10-05T15:45:16Z
updated_at: 2025-10-08T13:46:11Z
url: https://github.com/astral-sh/uv/pull/16125
synced_at: 2026-01-12T16:12:06Z
```

# Support `UV_WORKING_DIRECTORY` for setting `--directory`

---

_@shunsock_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This pull request enables the `--directory` option to accept environment variable: `UV_DIRECTORY`

### Motivation

Currently, the `--project` option already supports environment variables, but --directory does not.

The motivation for this change is the same as for the --project option. When using this option, it’s likely that the project root and the directory containing the uv project differ. In such cases, allowing environment variables makes it easier to avoid repeatedly specifying the directory in commands or task runners.

### Other PRs

- PR for create `--project` option: https://github.com/astral-sh/uv/pull/12327

## Test Plan

<!-- How was it tested? -->

### no auto testing

As with the --project option, no auto tests are included for this change.
This is because the implementation relies on Clap’s built-in attribute functionality, and testing such behavior would effectively mean testing a third-party crate, which would be redundant.

As long as the compiler accepts it, things should work as expected.

### testing manually

i tested manually like [previous pull request](https://github.com/astral-sh/uv/pull/12327)

```shell
$ cargo build --locked
./target/debug/uv init uv_directory

$ mkdir uv_directory

$ UV_DIRECTORY=uv_directory ./target/debug/uv sync
Using CPython 3.14.0rc3
Creating virtual environment at: .venv
Resolved 1 package in 15ms
Audited in 0.04ms

$ UV_DIRECTORY=uv_directory ./target/debug/uv run main.py
Hello from uv-directory!

$ ./target/debug/uv run main.py
error: Failed to spawn: `main.py`
  Caused by: No such file or directory (os error 2)
```

---

_Comment by @zanieb on 2025-10-06 15:36_

I think we might want this to be `UV_WORKING_DIRECTORY` if we add it? `UV_DIRECTORY` feels too ambiguous.

---

_Comment by @shunsock on 2025-10-06 16:01_

Thank you for reading. I agree with you. I'll rename it and fix the failing CI. I plan to make the changes by Thursday (JST).

---

_Comment by @shunsock on 2025-10-06 16:27_

I rename `UV_DIRECTORY` to `UV_WORKING_DIRECTORY`, and tested.

```shell
$ cargo build --locked
...
Finished `dev` profile [unoptimized + debuginfo] target(s) in 1m 06s

$ UV_WORKING_DIRECTORY=uv_directory ./target/debug/uv sync
Resolved 1 package in 12ms
Audited in 0.00ms

$ UV_WORKING_DIRECTORY=uv_directory ./target/debug/uv run main.py
Hello from uv-directory!

$ ./target/debug/uv run main.py
error: Failed to spawn: `main.py`
  Caused by: No such file or directory (os error 2)
```

---

_@zanieb approved on 2025-10-07 14:53_

---

_Comment by @zanieb on 2025-10-07 14:53_

You'll need to fix CI before this can be merged. Otherwise LGTM

---

_Label `configuration` added by @zanieb on 2025-10-07 14:54_

---

_Comment by @shunsock on 2025-10-07 16:32_

I fixed CI except [snapshot test](https://github.com/astral-sh/uv/actions/runs/18318439671/job/52165021439?pr=16125#step:11:5364) (`-\n` to `+\n`). Is there a way to update the snapshot?

---

_Comment by @shunsock on 2025-10-08 05:32_

Thank you for updating snapshots 👍 

---

_Renamed from "feature: option --directory allow to use environment variable" to "Support `UV_WORKING_DIRECTORY` for setting `--directory`" by @konstin on 2025-10-08 06:30_

---

_Merged by @zanieb on 2025-10-08 13:46_

---

_Closed by @zanieb on 2025-10-08 13:46_

---
