```yaml
number: 4613
title: Update README.md for PyPI Documentation Compatibility
type: pull_request
state: closed
author: KPCOFGS
labels: []
assignees: []
draft: true
base: main
head: main
created_at: 2024-06-28T12:06:47Z
updated_at: 2024-06-28T14:05:21Z
url: https://github.com/astral-sh/uv/pull/4613
synced_at: 2026-01-10T13:48:28Z
```

# Update README.md for PyPI Documentation Compatibility

---

_Pull request opened by @KPCOFGS on 2024-06-28 12:06_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Modified relative paths to absolute paths for PyPI compatibility, so PyPI can properly redirecting users to the correct place when some links are clicked

## Test Plan

Verified the links


---

_@charliermarsh reviewed on 2024-06-28 13:31_

I'm tempted to instead rewrite these prior to publishing, and to insert links to the exact version that we're building... We could do it in `transform_readme.md`.

---

_Converted to draft by @KPCOFGS on 2024-06-28 13:36_

---

_Closed by @KPCOFGS on 2024-06-28 14:05_

---
