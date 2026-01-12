```yaml
number: 20571
title: "[ty] Ecosystem analyzer: timing report"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/ecosystem-analyzer-timing
created_at: 2025-09-25T12:00:55Z
updated_at: 2025-09-25T12:14:23Z
url: https://github.com/astral-sh/ruff/pull/20571
synced_at: 2026-01-12T15:57:05Z
```

# [ty] Ecosystem analyzer: timing report

---

_@sharkdp_

## Summary

Generate a timing diff across the whole ecosystem and deploy it to CloudFlare pages. The timing information is collected already, we just need to create and upload the HTML report.

The timing results are just based on a single run. No statistical analysis across multiple runs or similar is performed. This means that results can be noisy, as can be seen on this PR, where we see slowdowns up to 1.26× and speedups down to 0.89×, even though the change should be neutral. Across all projects, these random events cancel out and we see an average factor of 1.01×. So I think this feature can still be interesting, given that it comes "for free". We just need to keep in mind that it will be noisy, and shouldn't read too much into these results.

## Test Plan

CI run on this PR (see the new *timing results* link).

---

_Label `ci` added by @sharkdp on 2025-09-25 12:01_

---

_Label `ty` added by @sharkdp on 2025-09-25 12:01_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-25 12:01_

---

_Comment by @github-actions[bot] on 2025-09-25 12:06_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://david-ecosystem-analyzer-tim.ecosystem-663.pages.dev/diff)** ([timing results](https://david-ecosystem-analyzer-tim.ecosystem-663.pages.dev/timing))


---

_Marked ready for review by @sharkdp on 2025-09-25 12:12_

---

_Comment by @sharkdp on 2025-09-25 12:14_

I will merge this to test it on a few branches. If it turns out to be more annoying than helpful, we can easily remove it.

---

_Merged by @sharkdp on 2025-09-25 12:14_

---

_Closed by @sharkdp on 2025-09-25 12:14_

---

_Branch deleted on 2025-09-25 12:14_

---
