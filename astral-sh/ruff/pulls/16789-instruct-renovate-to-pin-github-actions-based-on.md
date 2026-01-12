```yaml
number: 16789
title: Instruct Renovate to pin GitHub Actions based on SHA
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: micha/renovate-pin
created_at: 2025-03-17T07:29:38Z
updated_at: 2025-03-17T07:51:59Z
url: https://github.com/astral-sh/ruff/pull/16789
synced_at: 2026-01-12T15:55:57Z
```

# Instruct Renovate to pin GitHub Actions based on SHA

---

_@MichaReiser_

## Summary

The intent here is that all actions should be pinned to an immutable SHA (but that Renovate should annotate each SHA with the corresponding SemVer version).

See https://github.com/astral-sh/uv/pull/12189

## Test plan

```
npx --yes --package renovate -- renovate-config-validator
npm warn deprecated inflight@1.0.6: This module is not supported, and leaks memory. Do not use it. Check out lru-cache if you want a good and tested way to coalesce async requests by a key value, which is much more comprehensive and powerful.
npm warn deprecated rimraf@2.4.5: Rimraf versions prior to v4 are no longer supported
npm warn deprecated boolean@3.2.0: Package no longer supported. Contact Support at https://www.npmjs.com/support for more info.
npm warn deprecated glob@6.0.4: Glob versions prior to v9 are no longer supported
 INFO: Validating .github/renovate.json5
 INFO: Config validated successfully

```

---

_Label `ci` added by @MichaReiser on 2025-03-17 07:31_

---

_Merged by @MichaReiser on 2025-03-17 07:44_

---

_Closed by @MichaReiser on 2025-03-17 07:45_

---

_Branch deleted on 2025-03-17 07:45_

---

_Comment by @github-actions[bot] on 2025-03-17 07:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
