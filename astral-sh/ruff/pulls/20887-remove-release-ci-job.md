```yaml
number: 20887
title: "Remove `release` CI job"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: micha/larger-release-runner
created_at: 2025-10-15T10:05:28Z
updated_at: 2025-10-15T12:23:09Z
url: https://github.com/astral-sh/ruff/pull/20887
synced_at: 2026-01-12T15:57:11Z
```

# Remove `release` CI job

---

_@MichaReiser_

## Summary

We added the release CI job to catch issues with conditionally compiled code (`debug`, `cfg(debug_assertions)` back in April 2024 (https://github.com/astral-sh/ruff/pull/11182). 

But, we've since then also added `cargo-test-linux-release` (https://github.com/astral-sh/ruff/pull/14604), which not only compiles with the release profile but also runs all tests

https://github.com/astral-sh/ruff/blob/3d2ece2f42f24b0c18968d3a624d37eeb37525ff/.github/workflows/ci.yaml#L300-L331

That's why I think we can safely remove the release CI Job, as it's covered by cargo-test-linux-release

## Test Plan



---

_Label `ci` added by @MichaReiser on 2025-10-15 10:05_

---

_Label `release` added by @MichaReiser on 2025-10-15 10:07_

---

_Renamed from "Use larger runner for release build" to "Use profile `profiling` for release CI check" by @MichaReiser on 2025-10-15 10:09_

---

_Label `release` removed by @MichaReiser on 2025-10-15 10:13_

---

_Renamed from "Use profile `profiling` for release CI check" to "Remove `release` CI job" by @MichaReiser on 2025-10-15 10:14_

---

_Comment by @sharkdp on 2025-10-15 10:16_

> That's why I think we can safely remove the release CI Job, as it's covered by cargo-test-linux-release

Looks like the release job was running on macOS though, so we lose *some* coverage with this change. That might be acceptable if we have other macOS builds, though.

---

_Comment by @MichaReiser on 2025-10-15 10:22_

> Looks like the release job was running on macOS though, so we lose some coverage with this change. That might be acceptable if we have other macOS builds, though.

As far as I understand, the reason it ran on macOS was that those used to be the fastest runners available. We can introduce a new CI job that runs all tests on macos if we want, that should still be much cheaper than another release job

---

_@sharkdp approved on 2025-10-15 10:38_

---

_Merged by @MichaReiser on 2025-10-15 10:39_

---

_Closed by @MichaReiser on 2025-10-15 10:39_

---

_Branch deleted on 2025-10-15 10:39_

---

_Comment by @github-actions[bot] on 2025-10-15 10:42_

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

_@AlexWaygood reviewed on 2025-10-15 11:56_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:366 on 2025-10-15 11:56_

the MacOS job runs on Windows?

---

_@MichaReiser reviewed on 2025-10-15 12:23_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:366 on 2025-10-15 12:23_

yes, it's faster that way

---
