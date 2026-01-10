```yaml
number: 20626
title: "[ty] Ecosystem analyzer: relax timeout thresholds"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: david/ecosystem-analyzer-relax-timeout-thresholds
created_at: 2025-09-29T11:32:21Z
updated_at: 2025-09-29T11:36:15Z
url: https://github.com/astral-sh/ruff/pull/20626
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Ecosystem analyzer: relax timeout thresholds

---

_Pull request opened by @sharkdp on 2025-09-29 11:32_

## Summary

Pull in a small upstream change (https://github.com/astral-sh/ecosystem-analyzer/commit/6ce3a609575bc84eaf5d247739529c60b6c2ae5b), because some type check times were close to the previous limits, which prevents us from seeing diagnostics diffs (in case they run into a timeout).


---

_Label `ci` added by @sharkdp on 2025-09-29 11:32_

---

_Label `ty` added by @sharkdp on 2025-09-29 11:32_

---

_Merged by @sharkdp on 2025-09-29 11:36_

---

_Closed by @sharkdp on 2025-09-29 11:36_

---

_Branch deleted on 2025-09-29 11:36_

---
