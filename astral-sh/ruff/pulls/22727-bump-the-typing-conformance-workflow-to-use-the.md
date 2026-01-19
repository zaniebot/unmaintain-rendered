```yaml
number: 22727
title: "Bump the typing conformance workflow to use the latest `python/typing` commit"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/update-conformance-commit
created_at: 2026-01-19T16:16:50Z
updated_at: 2026-01-19T16:20:38Z
url: https://github.com/astral-sh/ruff/pull/22727
synced_at: 2026-01-19T16:27:07Z
```

# Bump the typing conformance workflow to use the latest `python/typing` commit

---

_@AlexWaygood_

## Summary

Using the commit we currently have pinned in CI, I see these conformance results if I run `uv run --no-project scripts/conformance.py --old-ty=./target/debug/ty --new-ty=./target/debug/ty --tests-path=../typing/conformance --force-summary-table` locally:

---

## [Typing conformance results](https://github.com/python/typing/blob/main/conformance/)

The percentage of diagnostics emitted that were expected errors held steady at 71.78%. The percentage of expected errors that received a diagnostic held steady at 60.22%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 669 | 669 | +0 |  |
| False Positives | 263 | 263 | +0 |  |
| False Negatives | 442 | 442 | +0 |  |
| Total Diagnostics | 932 | 932 | +0 |  |
| Precision | 71.78% | 71.78% | +0.00% |  |
| Recall | 60.22% | 60.22% | +0.00% |  |

---

With the new commit, I instead see these results:

---

## [Typing conformance results](https://github.com/python/typing/blob/main/conformance/)

The percentage of diagnostics emitted that were expected errors held steady at 77.13%. The percentage of expected errors that received a diagnostic held steady at 59.50%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 661 | 661 | +0 |  |
| False Positives | 196 | 196 | +0 |  |
| False Negatives | 450 | 450 | +0 |  |
| Total Diagnostics | 857 | 857 | +0 |  |
| Precision | 77.13% | 77.13% | +0.00% |  |
| Recall | 59.50% | 59.50% | +0.00% |  |

---

## Test Plan

The above commands I ran locally, + CI on this PR


---

_Label `ci` added by @AlexWaygood on 2026-01-19 16:16_

---

_Label `ty` added by @AlexWaygood on 2026-01-19 16:16_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 16:18_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Merged by @AlexWaygood on 2026-01-19 16:20_

---

_Closed by @AlexWaygood on 2026-01-19 16:20_

---

_Branch deleted on 2026-01-19 16:20_

---
