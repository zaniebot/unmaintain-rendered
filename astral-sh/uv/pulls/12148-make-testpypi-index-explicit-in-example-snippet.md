```yaml
number: 12148
title: Make testpypi index explicit in example snippet
type: pull_request
state: merged
author: ejohnso49
labels:
  - documentation
assignees: []
merged: true
base: main
head: ejohnso49/guide-make-testpypi-explicit
created_at: 2025-03-13T04:07:59Z
updated_at: 2025-03-13T23:23:10Z
url: https://github.com/astral-sh/uv/pull/12148
synced_at: 2026-01-12T16:10:08Z
```

# Make testpypi index explicit in example snippet

---

_@ejohnso49_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Update this example snippet adding test.pypi.org as a publishing index to mark the index with `explicit = true`. This will help prevent users from unexpected behavior if no other indices are defined and users don't select a different index selection algorithm (with `--index-strategy`). When `test.pypi.org` is the selected index for package management, packages resolve to odd versions like 0.0.1 and `uv` spits out lots of errors.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

N/A, documentation only change
<!-- How was it tested? -->


---

_Comment by @ejohnso49 on 2025-03-13 04:11_

I put up this PR because I ran into the same issue described in this [comment](https://github.com/astral-sh/uv/issues/12050#issuecomment-2706954004)

I was able to resolve with the suggested change [here](https://github.com/astral-sh/uv/issues/12050#issuecomment-2706924459) so figured it might be good to update the docs. Thanks!

---

_@charliermarsh approved on 2025-03-13 23:23_

This seems reasonable, thanks.

---

_Merged by @charliermarsh on 2025-03-13 23:23_

---

_Closed by @charliermarsh on 2025-03-13 23:23_

---

_Label `documentation` added by @charliermarsh on 2025-03-13 23:23_

---
