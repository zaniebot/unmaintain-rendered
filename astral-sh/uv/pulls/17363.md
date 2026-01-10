```yaml
number: 17363
title: Fix missing dependencies on synthetic root in SBOM export
type: pull_request
state: merged
author: thomasschafer
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-root-missing-in-all-packages-sbom-export
created_at: 2026-01-08T16:51:26Z
updated_at: 2026-01-08T18:19:34Z
url: https://github.com/astral-sh/uv/pull/17363
synced_at: 2026-01-10T05:49:14Z
```

# Fix missing dependencies on synthetic root in SBOM export

---

_Pull request opened by @thomasschafer on 2026-01-08 16:51_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

In [this PR](https://github.com/astral-sh/uv/pull/16523/), which added SBOM export functionality, we decided to add a synthetic root when using the `--all-packages` flag to ensure that there were no dangling workspace packages (i.e. workspace components that were not reachable from the root component). However, there is a bug here when not exporting a workspace, as the synthetic root does not depend on the project root. This PR fixes that and adds a test to verify the fix.

## Test Plan

Both tested manually and using the automated test added.


---

_Review requested from @woodruffw by @konstin on 2026-01-08 17:08_

---

_@woodruffw approved on 2026-01-08 17:26_

Thanks @thomasschafer!

---

_Label `bug` added by @konstin on 2026-01-08 18:10_

---

_Merged by @woodruffw on 2026-01-08 18:19_

---

_Closed by @woodruffw on 2026-01-08 18:19_

---
