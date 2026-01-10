```yaml
number: 16553
title: "Add `MACOSX_DEPLOYMENT_TARGET` hint for macos platform wheels"
type: pull_request
state: open
author: TaKO8Ki
labels: []
assignees: []
base: main
head: add-MACOSX_DEPLOYMENT_TARGET-hint
created_at: 2025-11-02T21:13:58Z
updated_at: 2025-11-28T18:53:48Z
url: https://github.com/astral-sh/uv/pull/16553
synced_at: 2026-01-10T05:49:14Z
```

# Add `MACOSX_DEPLOYMENT_TARGET` hint for macos platform wheels

---

_Pull request opened by @TaKO8Ki on 2025-11-02 21:13_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes  #10699

Since #11783 hadn’t been merged yet, I fixed the issues mentioned in the comments and opened this new pull request.

## Test Plan

<!-- How was it tested? -->

Added `invalid_platform_macos` to `pip_compile.rs`.

---
