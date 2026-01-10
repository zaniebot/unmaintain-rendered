```yaml
number: 14741
title: "Update `setup-uv` docs for Github Actions integration guide (re-order python and uv setup)"
type: pull_request
state: merged
author: alichaudry
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/fix-github-actions-python-uv-order
created_at: 2025-07-18T20:51:42Z
updated_at: 2025-07-21T20:46:34Z
url: https://github.com/astral-sh/uv/pull/14741
synced_at: 2026-01-10T06:53:02Z
```

# Update `setup-uv` docs for Github Actions integration guide (re-order python and uv setup)

---

_Pull request opened by @alichaudry on 2025-07-18 20:51_

I updated the Github Actions integration guide to run Github's `setup-python` before Astral's `setup-uv`, as `setup-uv`'s `activate-environment: true` doesn't work with the original ordering. There is a discussion about this behavior in the `setup-uv` repo [here](https://github.com/astral-sh/setup-uv/issues/479).

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Update the documentation for the Github Actions integration. Caveat: I'm unsure if there are any other reasons where the original ordering (that is,`setup-uv` before `setup-python`) might be preferred.

## Test Plan

Tested in a private Github Actions push, as documented in the aforementioned discussion on `setup-uv`'s repo. Confirmed that removing `source .venv/bin/activate` and replacing it with `activate-environment: true` now works in this ordering (but didn't work with the original ordering where `uv` installs before Github's `python`).


---

_Assigned to @zanieb by @zanieb on 2025-07-19 00:57_

---

_@zanieb approved on 2025-07-21 19:48_

Thanks!

---

_Merged by @zanieb on 2025-07-21 19:48_

---

_Closed by @zanieb on 2025-07-21 19:48_

---

_Label `documentation` added by @zanieb on 2025-07-21 19:48_

---

_Branch deleted on 2025-07-21 20:46_

---
