```yaml
number: 9839
title: Implement pip list --verbose
type: pull_request
state: open
author: julienp
labels: []
assignees: []
draft: true
base: main
head: main
created_at: 2024-12-12T13:11:52Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/9839
synced_at: 2026-01-10T11:10:34Z
```

# Implement pip list --verbose

---

_Pull request opened by @julienp on 2024-12-12 13:11_

## Summary

Handle the `-v/--verbose` flag for `pip list`.

Fixes https://github.com/astral-sh/uv/issues/9838


---

_@julienp reviewed on 2024-12-12 13:14_

---

_Review comment by @julienp on `crates/uv/src/commands/pip/list.rs`:161 on 2024-12-12 13:14_

Pip lists the location where the package is installed, for example `[SITE_PACKAGES]`, whereas `dist.path()` is something like `[SITE_PACKAGES]/pulumi-3.143.0.dist-info`.

The `parent()` bit mostly works for this (although we get a trailing `/`), but it's probably not quite right for all types of installations.

---

_Review comment by @julienp on `crates/uv/src/commands/pip/list.rs`:153 on 2024-12-12 13:16_

How do we thread this through to the list command, and do we still want the verbosity flag to apply to `uv` in general?

---

_@julienp reviewed on 2024-12-12 13:16_

---

_Renamed from "Implement pip list —verbose" to "Implement pip list --verbose" by @julienp on 2024-12-12 13:16_

---
