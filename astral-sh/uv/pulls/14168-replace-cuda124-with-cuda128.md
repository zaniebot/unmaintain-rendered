```yaml
number: 14168
title: Replace cuda124 with cuda128
type: pull_request
state: merged
author: lvvittor
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-docs
created_at: 2025-06-20T19:29:23Z
updated_at: 2025-06-20T20:17:30Z
url: https://github.com/astral-sh/uv/pull/14168
synced_at: 2026-01-12T16:11:03Z
```

# Replace cuda124 with cuda128

---

_@lvvittor_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Replace wrong `cuda124` version to the correct `cuda128` version in torch docs

## Test Plan

<!-- How was it tested? -->


---

_Comment by @zanieb on 2025-06-20 19:38_

Can you explain why cuda124 is wrong?

---

_Comment by @lvvittor on 2025-06-20 20:03_

In the docs [Configuring accelerators with optional dependencies](https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-optional-dependencies), the extra `cu124` is not defined, but `cu128` is used in the example `pyproject.toml`. When `uv sync` is run with an `--extra` flag that doesn't correspond to a defined group in the `pyproject.toml`, uv silently ignores the unknown extra (`cu124` in this case).



---

_Comment by @charliermarsh on 2025-06-20 20:05_

I think this is right -- thanks.

---

_Merged by @charliermarsh on 2025-06-20 20:05_

---

_Closed by @charliermarsh on 2025-06-20 20:05_

---

_Label `documentation` added by @charliermarsh on 2025-06-20 20:05_

---

_Comment by @charliermarsh on 2025-06-20 20:05_

(Must've been an oversight when I upgraded to `cu128`.)

---

_Comment by @zanieb on 2025-06-20 20:17_

Thanks!

---
