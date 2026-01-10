```yaml
number: 3523
title: "ci: build musllinux_1_1 wheels"
type: pull_request
state: merged
author: henryiii
labels:
  - releases
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-05-11T04:59:56Z
updated_at: 2024-05-11T16:36:20Z
url: https://github.com/astral-sh/uv/pull/3523
synced_at: 2026-01-10T14:37:54Z
```

# ci: build musllinux_1_1 wheels

---

_Pull request opened by @henryiii on 2024-05-11 04:59_

This is needed to include uv in manylinux, which has both the musllinux_1_1 and musllinux_1_2 images.

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

We are looking into including uv in the `pypa/manylinux` docker images, so that cibuildwheel will be able to use it. But to do so, we need all actively supported images to be able to take it, including musllinux_1_1.

## Test Plan

The test was updated to make sure Alpine 3.12, which musllinux was based on, can run uv.

The binary build does need to be triggered on this PR to make sure it works. Ah, https://github.com/astral-sh/uv/blob/5b6040184de9be1813cde1ff6c55762ee304ad99/.github/workflows/build-binaries.yml#L15-L20 is a good idea, nice.

Will need a followup to fill out the other platforms `pypa/manylinux` supports for musllinux.


---

_Converted to draft by @henryiii on 2024-05-11 05:03_

---

_Comment by @henryiii on 2024-05-11 05:03_

(Making this draft because I forgot the cross-compiled musl wheels. I'll wait to see if it works first)

---

_@henryiii reviewed on 2024-05-11 05:56_

---

_Review comment by @henryiii on `.github/workflows/build-binaries.yml`:522 on 2024-05-11 05:56_

```suggestion
            python3 -m venv .venv
            .venv/bin/pip install --upgrade pip
```

This pip didn't know about musllinux yet, obviously. :)

---

_Marked ready for review by @henryiii on 2024-05-11 06:06_

---

_@charliermarsh approved on 2024-05-11 16:35_

---

_Merged by @charliermarsh on 2024-05-11 16:35_

---

_Closed by @charliermarsh on 2024-05-11 16:35_

---

_Comment by @charliermarsh on 2024-05-11 16:35_

Awesome, thank you!

---

_Label `release` added by @charliermarsh on 2024-05-11 16:35_

---

_Branch deleted on 2024-05-11 16:36_

---
