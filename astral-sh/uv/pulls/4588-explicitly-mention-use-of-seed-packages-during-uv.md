```yaml
number: 4588
title: "Explicitly mention use of seed packages during `uv venv --seed`"
type: pull_request
state: merged
author: Peiffap
labels:
  - documentation
  - tracing
assignees: []
merged: true
base: main
head: venv/clarify-seed
created_at: 2024-06-27T14:16:28Z
updated_at: 2024-06-27T14:38:42Z
url: https://github.com/astral-sh/uv/pull/4588
synced_at: 2026-01-10T13:48:28Z
```

# Explicitly mention use of seed packages during `uv venv --seed`

---

_Pull request opened by @Peiffap on 2024-06-27 14:16_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

Closes #1329.

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Mentions use of seed packages during `uv venv --seed`, and clarifies the divergence in behavior when using Python 3.12+.

## Test Plan

<!-- How was it tested? -->

`cargo nextest run --test venv`


---

_Renamed from "Venv/clarify seed" to "Explicitly mention use of seed packages during `uv venv --seed`" by @Peiffap on 2024-06-27 14:25_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1502 on 2024-06-27 14:27_

```suggestion
    /// Note `setuptools` and `wheel` are not included in Python 3.12+ environments.
```

---

_@zanieb approved on 2024-06-27 14:27_

Thank you!

---

_Label `documentation` added by @zanieb on 2024-06-27 14:27_

---

_Label `tracing` added by @zanieb on 2024-06-27 14:27_

---

_Comment by @Peiffap on 2024-06-27 14:31_

Thanks for always getting to stuff so quickly!

---

_Merged by @zanieb on 2024-06-27 14:36_

---

_Closed by @zanieb on 2024-06-27 14:36_

---

_Branch deleted on 2024-06-27 14:38_

---
