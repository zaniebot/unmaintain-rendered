```yaml
number: 16521
title: "Don't use UV_LOCKED to enable `--check` flag"
type: pull_request
state: merged
author: AlexVndnblcke
labels: []
assignees: []
merged: true
base: main
head: 16517
created_at: 2025-10-30T18:08:28Z
updated_at: 2025-10-30T19:45:50Z
url: https://github.com/astral-sh/uv/pull/16521
synced_at: 2026-01-10T06:28:12Z
```

# Don't use UV_LOCKED to enable `--check` flag

---

_Pull request opened by @AlexVndnblcke on 2025-10-30 18:08_

Env var UV_LOCKED should only be used to enable `--locked` for the `uv lock` command. Previously `--check` was also enabled by specifying UV_LOCKED.


<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Prevent environment variable `UV_LOCKED` from enabling both `--check` and `--locked` in `uv lock`. This resulted in issues since `--check` and `--locked` are conflicting.

## Test Plan

<!-- How was it tested? -->
I tested the result with `UV_LOCKED=true uv lock` to see if the cli error didn't occur.

Closes #16517

---

_@zanieb approved on 2025-10-30 18:35_

Thanks!

---

_Merged by @charliermarsh on 2025-10-30 19:37_

---

_Closed by @charliermarsh on 2025-10-30 19:37_

---

_Branch deleted on 2025-10-30 19:45_

---
