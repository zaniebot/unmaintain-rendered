```yaml
number: 21374
title: "[`flake8-bandit`] Support new PySNMP API paths (`S508`, `S509`)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-21364
created_at: 2025-11-11T04:34:00Z
updated_at: 2025-11-19T20:03:23Z
url: https://github.com/astral-sh/ruff/pull/21374
synced_at: 2026-01-12T15:57:22Z
```

# [`flake8-bandit`] Support new PySNMP API paths (`S508`, `S509`)

---

_@danparizher_

## Summary

Updated `S508` (snmp-insecure-version) and `S509` (snmp-weak-cryptography) rules to support both old and new PySNMP API module paths. Previously, these rules only detected the old API path `pysnmp.hlapi.*`, but now they correctly detect all PySNMP API variants including `pysnmp.hlapi.asyncio.*`, `pysnmp.hlapi.v1arch.*`, `pysnmp.hlapi.v3arch.*`, and `pysnmp.hlapi.auth.*`.

Fixes #21364

## Problem Analysis

The `S508` and `S509` rules used exact pattern matching on qualified names:
- `S509` only matched `["pysnmp", "hlapi", "UsmUserData"]`
- `S508` only matched `["pysnmp", "hlapi", "CommunityData"]`

This meant that newer PySNMP API paths were not detected, such as:
- `pysnmp.hlapi.asyncio.UsmUserData`
- `pysnmp.hlapi.v3arch.asyncio.UsmUserData`
- `pysnmp.hlapi.v3arch.asyncio.auth.UsmUserData`
- `pysnmp.hlapi.auth.UsmUserData`
- Similar variants for `CommunityData` in `S508`

Additionally, the old API path `pysnmp.hlapi.auth.*` was also missing from both rules.

## Approach

Instead of exact pattern matching, both rules now check if:
1. The qualified name starts with `["pysnmp", "hlapi"]`
2. The qualified name ends with the target class name (`"UsmUserData"` for `S509`, `"CommunityData"` for `S508`)

This flexible approach matches all PySNMP API paths without hardcoding each variant, making the rules more maintainable and future-proof.

## Test Plan

Added comprehensive test cases to both `S508.py` and `S509.py` test files covering:
- New API paths: `pysnmp.hlapi.asyncio.*`, `pysnmp.hlapi.v1arch.*`, `pysnmp.hlapi.v3arch.*`
- Old API path: `pysnmp.hlapi.auth.*`
- Both insecure and secure usage patterns

All existing tests pass, and new snapshot tests were added and accepted. Manual verification confirms both rules correctly detect all PySNMP API variants.

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 04:46_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bandit/rules/snmp_insecure_version.rs`:54 on 2025-11-13 22:44_

I think we can still use `matches!` here:

```suggestion
           matches!(
           		qualified_name.segments(),
            	["pysnmp", "hlapi", .., "CommunityData"]
           )
```

This is slightly more restrictive than it really needs to be, but it's a lot shorter than enumerating all of the options for what `..` can be, especially when different path lengths are involved.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bandit/rules/snmp_weak_cryptography.rs`:50 on 2025-11-13 22:47_

Same as above, let's stick with `matches!`.

---

_@ntBre requested changes on 2025-11-13 22:58_

Thanks! I had one suggestion about simplifying the `segments` checks, and let's make this a preview change just to be safe since these are stable rules.

---

_Review requested from @ntBre by @danparizher on 2025-11-13 23:49_

---

_Label `rule` added by @ntBre on 2025-11-19 19:37_

---

_Label `preview` added by @ntBre on 2025-11-19 19:37_

---

_@ntBre approved on 2025-11-19 19:50_

Thanks!

I just reverted the removal of the stable tests and reused the existing `preview_rules` test runner for these two rules.

---

_Merged by @ntBre on 2025-11-19 20:03_

---

_Closed by @ntBre on 2025-11-19 20:03_

---
