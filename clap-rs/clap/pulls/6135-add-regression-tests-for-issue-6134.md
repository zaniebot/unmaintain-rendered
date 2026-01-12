```yaml
number: 6135
title: "add regression tests for issue #6134"
type: pull_request
state: closed
author: vjgaur
labels: []
assignees: []
base: master
head: master
created_at: 2025-09-19T12:42:29Z
updated_at: 2025-09-19T17:05:30Z
url: https://github.com/clap-rs/clap/pull/6135
synced_at: 2026-01-12T16:14:18Z
```

# add regression tests for issue #6134

---

_@vjgaur_

## Summary
Adds regression tests for issue [#6134](https://github.com/clap-rs/clap/issues/6134) to document the expected behavior when positional arguments start with dashes.

## Background
Issue [#6134](https://github.com/clap-rs/clap/issues/6134) reported that arguments like `"-foo"` fail when passed as positional arguments. This is the intended behavior - clap treats anything starting with `-` as a potential flag by default.

## Changes
- Added two tests to `tests/builder/app_settings.rs`:
  - `issue_6134_positional_with_leading_dash_fails_without_allow_hyphen_values()` - Documents that the reported behavior is expected
  - `issue_6134_positional_with_leading_dash_works_with_allow_hyphen_values()` - Shows the correct solution using `allow_hyphen_values = true`

## Resolution
These tests confirm that issue #6134 is not a bug but expected behavior. Users should use `allow_hyphen_values = true` on arguments that need to accept dash-prefixed values.

Closes [#6134](https://github.com/clap-rs/clap/issues/6134)


---
