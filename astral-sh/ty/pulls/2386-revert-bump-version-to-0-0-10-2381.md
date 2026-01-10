```yaml
number: 2386
title: "Revert \"Bump version to 0.0.10 (#2381)\""
type: pull_request
state: closed
author: Gankra
labels: []
assignees: []
base: main
head: gankra/try-old
created_at: 2026-01-07T22:59:08Z
updated_at: 2026-01-07T23:29:05Z
url: https://github.com/astral-sh/ty/pull/2386
synced_at: 2026-01-10T02:34:11Z
```

# Revert "Bump version to 0.0.10 (#2381)"

---

_Pull request opened by @Gankra on 2026-01-07 22:59_

This reverts commit d18902cdcc4d39badfad86c88e89b866a809c881.

<!--
Thank you for contributing to ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @carljm on 2026-01-07 23:09_

Not sure I followed all the discussion on this, but is this what we actually want to do? Seems like we got the wheels all uploaded and 0.0.10 is live on PyPI now.

---

_Comment by @charliermarsh on 2026-01-07 23:10_

I think the purpose here was to assess whether the PPC64 wheel was built with or without a manylinux tag after rolling back to v0.0.9 (to determine whether the issue was in our code or an unpinned dependency).

---

_Comment by @Gankra on 2026-01-07 23:29_

And indeed it was in an unpinned dependency

---

_Closed by @Gankra on 2026-01-07 23:29_

---
