```yaml
number: 9315
title: "Allow building multiple packages in one `uv build` run"
type: pull_request
state: open
author: filmor
labels: []
assignees: []
draft: true
base: main
head: multiple-package-build
created_at: 2024-11-21T13:32:40Z
updated_at: 2025-04-14T08:00:32Z
url: https://github.com/astral-sh/uv/pull/9315
synced_at: 2026-01-12T16:08:45Z
```

# Allow building multiple packages in one `uv build` run

---

_@filmor_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Allow `uv build --package pkg1 --package pkg2`.


## Test Plan

I have run this by hand on a local workspace, but will add tests if the change is accepted in principle :)

---

_Comment by @charliermarsh on 2024-11-26 23:03_

This seems okay to me... It's inconsistent with other interfaces, but it does kind of make sense here whereas it doesn't elsewhere. I'd like a second opinion from @zanieb or @konstin though.

---

_Comment by @konstin on 2024-11-26 23:07_

I think this is a good feature, if we allow a single package and `--all-packages`, it makes sense to also allow multiple, but not all packages.

---
