```yaml
number: 21196
title: "[ty] Increase timeout-minutes to 10 for py-fuzzer job"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: fuzz-timeout
created_at: 2025-11-02T01:34:25Z
updated_at: 2025-11-02T02:06:04Z
url: https://github.com/astral-sh/ruff/pull/21196
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Increase timeout-minutes to 10 for py-fuzzer job

---

_Pull request opened by @AlexWaygood on 2025-11-02 01:34_

In https://github.com/astral-sh/ruff/pull/20937, we reduced this from 20 to 5, so that it would become obvious if ty was now taking a pathological amount of time to type-check a given seed. 5 minutes is easily long enough if there are no new bugs, but it turns out to be too low if there _are_ new bugs, because on encountering a new bug the fuzzer must repeatedly run ty on a smaller and smaller snippet in order to try to minimize the reproducible example. That means that the fuzzer job is timing out on https://github.com/astral-sh/ruff/pull/20962, not because of pathological performance issues but because we're not giving the script enough time to minimize the bugs that it's found.

Hopefully 10 should be a good compromise where it still becomes obvious if we introduce pathological performance issues on certain fuzzer seeds, but we give the fuzzer enough time to minimize examples when it finds new bugs.

A better solution to find new pathological performance issues on fuzzer seeds might be to just compare the execution time between the baseline-executable run on a given seed and the test-executable run on the same seed. This is just a quick-and-easy fix for now.


---

_Label `ci` added by @AlexWaygood on 2025-11-02 01:34_

---

_Label `ty` added by @AlexWaygood on 2025-11-02 01:34_

---

_Comment by @github-actions[bot] on 2025-11-02 01:52_

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

_@MichaReiser approved on 2025-11-02 01:59_

---

_Comment by @sharkdp on 2025-11-02 02:01_

Thanks

---

_Merged by @AlexWaygood on 2025-11-02 02:06_

---

_Closed by @AlexWaygood on 2025-11-02 02:06_

---

_Branch deleted on 2025-11-02 02:06_

---
