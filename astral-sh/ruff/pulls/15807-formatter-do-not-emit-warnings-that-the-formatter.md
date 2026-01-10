```yaml
number: 15807
title: "[`formatter`] do not emit warnings that the formatter conflicts with rule `missing-trailing-comma` if no conflicts"
type: pull_request
state: closed
author: XuehaiPan
labels:
  - formatter
  - incompatibility
assignees: []
base: main
head: do-not-warn-non-conflicting-rules
created_at: 2025-01-29T09:17:38Z
updated_at: 2025-02-03T07:59:23Z
url: https://github.com/astral-sh/ruff/pull/15807
synced_at: 2026-01-10T19:57:22Z
```

# [`formatter`] do not emit warnings that the formatter conflicts with rule `missing-trailing-comma` if no conflicts

---

_Pull request opened by @XuehaiPan on 2025-01-29 09:17_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Only emit a warning if the formatter's setting [skip-magic-trailing-comma](https://docs.astral.sh/ruff/settings/#format_skip-magic-trailing-comma) is conflicting with the linter rule [missing-trailing-comma (COM812)](https://docs.astral.sh/ruff/rules/missing-trailing-comma/#missing-trailing-comma-com812).

The following settings can co-exist with each other.

```toml
[lint]
select = [
    "COM",  # flake8-commas
]

[formatter]
skip-magic-trailing-comma = false  # the default value
```

## Test Plan

<!-- How was it tested? -->

CI

---

_Comment by @github-actions[bot] on 2025-01-29 09:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[`formatter`] do not emit warnings that the formatter conflicts with rule `missing-trailing-commas` if no conflicts" to "[`formatter`] do not emit warnings that the formatter conflicts with rule `missing-trailing-comma` if no conflicts" by @XuehaiPan on 2025-01-29 14:00_

---

_Label `formatter` added by @dylwil3 on 2025-01-30 14:00_

---

_Label `incompatibility` added by @MichaReiser on 2025-02-03 07:50_

---

_Comment by @MichaReiser on 2025-02-03 07:59_

Thanks for opening this PR. 

I don't think the incompatibility depends on the `skip-magic-trailing-comma` option as you can see in this [Playground](https://play.ruff.rs/c3d115ed-7e67-431f-8192-6ef3aa903fe2) and is discussed in https://github.com/astral-sh/ruff/issues/9216. 

COM812 complains about a missing trailing comma for the second statement which is the formatted version of the first statement. 

---

_Closed by @MichaReiser on 2025-02-03 07:59_

---
