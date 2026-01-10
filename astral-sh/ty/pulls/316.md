```yaml
number: 316
title: Ensure ty runs at latest version
type: pull_request
state: merged
author: soof-golan
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-05-11T11:43:34Z
updated_at: 2025-05-12T08:40:51Z
url: https://github.com/astral-sh/ty/pull/316
synced_at: 2026-01-10T02:34:10Z
```

# Ensure ty runs at latest version

---

_Pull request opened by @soof-golan on 2025-05-11 11:43_


<!--
Thank you for contributing to ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

teeny-tiny update to the README that I think will positively impact adoption of ty

## Summary

Update the 'front page' `uv tool install` command to use `@latest` to ensure users upgrade their cached ty version.

This is especially helpful while ty is not stable and fixes are pushed frequently

## Test Plan

<!-- How was it tested? -->

running `uv tool install ty@latest` successfully checks for updates and installs the latest available version of ty

```bash
$ uv tool install ty==0.0.0a7
`ty==0.0.0a7` is already installed
$ uv tool install ty@latest  
Resolved 1 package in 245ms
Prepared 1 package in 1ms
Uninstalled 1 package in 2ms
Installed 1 package in 3ms
 - ty==0.0.0a7
 + ty==0.0.0a8
Installed 1 executable: ty
```


---

_Label `documentation` added by @MichaReiser on 2025-05-12 06:40_

---

_@MichaReiser approved on 2025-05-12 06:40_

Thanks

---

_Merged by @MichaReiser on 2025-05-12 06:40_

---

_Closed by @MichaReiser on 2025-05-12 06:40_

---

_Branch deleted on 2025-05-12 08:40_

---
