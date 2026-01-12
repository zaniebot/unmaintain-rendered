```yaml
number: 18913
title: "[ty] Change `environment.root` to accept multiple paths"
type: pull_request
state: merged
author: MichaReiser
labels:
  - configuration
  - ty
assignees: []
merged: true
base: main
head: micha/root-multiple-paths
created_at: 2025-06-24T12:19:18Z
updated_at: 2025-06-24T12:52:38Z
url: https://github.com/astral-sh/ruff/pull/18913
synced_at: 2026-01-12T15:56:27Z
```

# [ty] Change `environment.root` to accept multiple paths

---

_@MichaReiser_

## Summary
Change `environment.root` to accept multiple paths to allow e.g. `root = ["./app", "./tests"]`.

I decided not to extend the syntax to support globs for now. I think that's something we can add later if it proves to be necessary. Supporting globs also has the downside that it requires a directory traversal to find all matches.

Closes https://github.com/astral-sh/ty/issues/179


## Test Plan

Updated tests


---

_Review requested from @carljm by @MichaReiser on 2025-06-24 12:19_

---

_Label `configuration` added by @MichaReiser on 2025-06-24 12:19_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-24 12:19_

---

_Label `ty` added by @MichaReiser on 2025-06-24 12:19_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-24 12:19_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-24 12:19_

---

_@AlexWaygood approved on 2025-06-24 12:21_

---

_Comment by @github-actions[bot] on 2025-06-24 12:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-06-24 12:28_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Froot-multiple-paths?runnerMode=Instrumentation)

### Merging #18913 will **not alter performance**

<sub>Comparing <code>micha/root-multiple-paths</code> (3b00b04) with <code>main</code> (0194452)</sub>



### Summary

`✅ 37` untouched benchmarks  





---

_Merged by @MichaReiser on 2025-06-24 12:52_

---

_Closed by @MichaReiser on 2025-06-24 12:52_

---

_Branch deleted on 2025-06-24 12:52_

---
