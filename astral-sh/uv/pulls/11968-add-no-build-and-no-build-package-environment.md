```yaml
number: 11968
title: "Add `NO_BUILD` and `NO_BUILD_PACKAGE` environment variables"
type: pull_request
state: merged
author: lengau
labels:
  - configuration
assignees: []
merged: true
base: main
head: 11963/no-build-env
created_at: 2025-03-05T01:59:59Z
updated_at: 2025-03-07T17:47:36Z
url: https://github.com/astral-sh/uv/pull/11968
synced_at: 2026-01-10T11:10:39Z
```

# Add `NO_BUILD` and `NO_BUILD_PACKAGE` environment variables

---

_Pull request opened by @lengau on 2025-03-05 01:59_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Similar to https://github.com/astral-sh/uv/pull/11399

This adds `UV_NO_BUILD` and `UV_NO_BUILD_PACKAGE` environment variables for non-pip commands.

## Test Plan

<!-- How was it tested? -->
Tested manually and with snapshot tests.


Fixes #11963

---

_@zanieb reviewed on 2025-03-05 04:57_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:4606 on 2025-03-05 04:57_

A bit of an aside, but this is pretty surprising! cc @charliermarsh 

---

_@zanieb approved on 2025-03-05 04:58_

Thank you again!

---

_Label `configuration` added by @zanieb on 2025-03-05 04:58_

---

_Merged by @zanieb on 2025-03-05 04:58_

---

_Closed by @zanieb on 2025-03-05 04:58_

---

_Branch deleted on 2025-03-05 05:02_

---

_Comment by @weystorm on 2025-03-07 17:24_

Should `UV_NO_BUILD` work for `uv pip install` commands as well?
I have a Linux host, where `gcc` is not available for certain reasons. Running below command still wants to invoke gcc upon installing:
`UV_NO_BUILD=1 uv pip install caio`

Whereas `uv pip install caio --no-build` works.

---

_Comment by @charliermarsh on 2025-03-07 17:26_

No, I don't believe those are respected in the `uv pip` CLI since the interface is a little different to match `pip`. E.g., in `uv pip`, you need to do `--no-build :all:`.

---

_Comment by @zanieb on 2025-03-07 17:35_

We want to thread these through to there, I think — we just haven't done it yet.

---

_Comment by @weystorm on 2025-03-07 17:47_

Oh, it also says right in the commit message, my bad. 🙂
> for non-pip commands.

---
