```yaml
number: 22086
title: Upload full ecosystem report as a GitHub Actions artifact
type: pull_request
state: merged
author: woodruffw
labels:
  - internal
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ww/upload-report-artifact
created_at: 2025-12-19T15:46:48Z
updated_at: 2025-12-19T16:41:23Z
url: https://github.com/astral-sh/ruff/pull/22086
synced_at: 2026-01-12T15:57:41Z
```

# Upload full ecosystem report as a GitHub Actions artifact

---

_@woodruffw_

## Summary

This is the first step towards removing the current in-action limitation with the report.

Once this is being consistently uploaded, I can begin to integrate it into astral-sh-bot, which'll then perform the appropriate Cloudflare Pages deployment directly.

## Test Plan

NFC. Intentionally running the ecosystem-analyzer to ensure this works 🙂 

---

_Review requested from @sharkdp by @woodruffw on 2025-12-19 15:46_

---

_Assigned to @woodruffw by @woodruffw on 2025-12-19 15:46_

---

_Label `ecosystem-analyzer` added by @woodruffw on 2025-12-19 15:46_

---

_Review requested from @AlexWaygood by @woodruffw on 2025-12-19 15:46_

---

_Label `internal` added by @woodruffw on 2025-12-19 15:46_

---

_Label `ecosystem-analyzer` removed by @woodruffw on 2025-12-19 15:49_

---

_Label `ecosystem-analyzer` added by @woodruffw on 2025-12-19 15:49_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 15:55_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 0 | 2 | 6 |
| `invalid-argument-type` | 2 | 0 | 1 |
| **Total** | **2** | **2** | **7** |

**[Full report with detailed diff](https://ww-upload-report-artifact.ecosystem-663.pages.dev/diff)** ([timing results](https://ww-upload-report-artifact.ecosystem-663.pages.dev/timing))




---

_@AlexWaygood approved on 2025-12-19 16:20_

---

_Merged by @woodruffw on 2025-12-19 16:41_

---

_Closed by @woodruffw on 2025-12-19 16:41_

---

_Branch deleted on 2025-12-19 16:41_

---
