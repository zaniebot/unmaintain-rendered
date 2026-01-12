```yaml
number: 19819
title: Feature/build riscv64 bin
type: pull_request
state: merged
author: ffgan
labels:
  - release
assignees: []
merged: true
base: main
head: feature/build_riscv64_bin
created_at: 2025-08-08T03:27:10Z
updated_at: 2025-08-17T23:48:58Z
url: https://github.com/astral-sh/ruff/pull/19819
synced_at: 2026-01-12T15:56:48Z
```

# Feature/build riscv64 bin

---

_@ffgan_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Recently, PyPI has added support for riscv64. Ruff can consider adding support for building Ruff on riscv64 so that Ruff can be used directly on riscv64.

## Test Plan

<!-- How was it tested? -->

Here are the CI [results](https://github.com/ffgan/ruff/actions/runs/16820981817/job/47647672573) from my fork.


## other info
Co-authored by: [nijincheng@iscas.ac.cn](mailto:nijincheng@iscas.ac.cn);


---

_Label `release` added by @ntBre on 2025-08-08 16:23_

---

_Comment by @MichaReiser on 2025-08-12 09:36_

Thank you. For reference, uv added riscv64 support in https://github.com/astral-sh/uv/pull/12688

@Gankra would you mind taking a look at this PR? The changes do make sense to me

---

_Comment by @github-actions[bot] on 2025-08-12 09:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@Gankra approved on 2025-08-14 13:50_

Looks right to me!

---

_Merged by @MichaReiser on 2025-08-14 14:11_

---

_Closed by @MichaReiser on 2025-08-14 14:11_

---

_Branch deleted on 2025-08-17 23:48_

---
