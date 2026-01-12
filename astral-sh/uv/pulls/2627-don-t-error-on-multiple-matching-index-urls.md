```yaml
number: 2627
title: "Don't error on multiple matching index URLs"
type: pull_request
state: merged
author: BakerNet
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/multiple-matching-index-url
created_at: 2024-03-22T22:27:56Z
updated_at: 2024-03-22T23:38:34Z
url: https://github.com/astral-sh/uv/pull/2627
synced_at: 2026-01-12T16:05:08Z
```

# Don't error on multiple matching index URLs

---

_@BakerNet_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Closes Issue:
- https://github.com/astral-sh/uv/issues/2626

## Test Plan

<!-- How was it tested? -->

```
cargo run -- pip install -r dev-requirements.txt -r requirements.txt
```
where both requirements files have same `--index-url`

---

_Comment by @zanieb on 2024-03-22 22:48_

Could you add a test case that just uses test PyPI as the URL or something?

---

_Label `bug` added by @zanieb on 2024-03-22 22:48_

---

_Comment by @BakerNet on 2024-03-22 23:13_

> Could you add a test case that just uses test PyPI as the URL or something?

Done

---

_@charliermarsh approved on 2024-03-22 23:29_

---

_Merged by @charliermarsh on 2024-03-22 23:37_

---

_Closed by @charliermarsh on 2024-03-22 23:37_

---

_Comment by @charliermarsh on 2024-03-22 23:38_

Thanks!

---
