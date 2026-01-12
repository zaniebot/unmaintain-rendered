```yaml
number: 14478
title: "Only build the `fuzz` crate on `main`"
type: pull_request
state: merged
author: zanieb
labels:
  - ci
assignees: []
merged: true
base: main
head: zb/fuzz-build
created_at: 2024-11-20T05:02:27Z
updated_at: 2024-11-20T06:57:26Z
url: https://github.com/astral-sh/ruff/pull/14478
synced_at: 2026-01-12T15:55:48Z
```

# Only build the `fuzz` crate on `main`

---

_@zanieb_

It's not actually doing anything per pull request and it's pretty slow? xref #14469 

It seems useful to build on `main` still to find build regressions? e.g. https://github.com/astral-sh/ruff/issues/9368

---

_Label `ci` added by @zanieb on 2024-11-20 05:02_

---

_Renamed from "Only build the `ruff` crate on `main`" to "Only build the `fuzz` crate on `main`" by @zanieb on 2024-11-20 05:03_

---

_Marked ready for review by @zanieb on 2024-11-20 05:05_

---

_@dhruvmanila approved on 2024-11-20 05:07_

Thanks!

---

_Merged by @zanieb on 2024-11-20 05:07_

---

_Closed by @zanieb on 2024-11-20 05:07_

---

_Branch deleted on 2024-11-20 05:07_

---

_Comment by @github-actions[bot] on 2024-11-20 05:12_

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

_Comment by @MichaReiser on 2024-11-20 06:57_

I generally found the fuzzer CI job useful because I never build the fuzzer locally. I often not even build the fuzzer locally when I break the build, instead I just push a fix and see if CI breaks now (because it requires some setup before you can build it successfully). That workflow no longer works. 

I have never felt blocked by the fuzzer build. The slowest job, but very high signal, is the ecosystem check. That's what I'm waiting for. 

---
