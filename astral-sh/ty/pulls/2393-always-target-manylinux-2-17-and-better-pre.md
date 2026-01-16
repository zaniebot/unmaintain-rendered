```yaml
number: 2393
title: Always target manylinux 2_17 and better pre-upload checks
type: pull_request
state: open
author: konstin
labels:
  - internal
assignees: []
base: main
head: konsti/manylinux_2_17
created_at: 2026-01-08T08:40:07Z
updated_at: 2026-01-16T09:05:02Z
url: https://github.com/astral-sh/ty/pull/2393
synced_at: 2026-01-16T10:07:16Z
```

# Always target manylinux 2_17 and better pre-upload checks

---

_@konstin_

We currently build for manylinux_2_17, and if this target changes, it should be an intentional, explicit change.

Closes #2389
Closes https://github.com/astral-sh/ty/issues/2396

---

_Comment by @konstin on 2026-01-08 09:32_

Test run: https://github.com/astral-sh/ty/actions/runs/20813401599?pr=2393

---

_Label `internal` added by @konstin on 2026-01-08 09:32_

---

_Comment by @MichaReiser on 2026-01-08 12:46_

I assume this requires the upstream maturin fix first or is the build failing for another reason?

---

_Comment by @konstin on 2026-01-08 13:30_

Yep, on it: https://github.com/PyO3/maturin/pull/2926

---

_Renamed from "Always target manylinux_2_17" to "Always target manylinux 2_17 and better pre-upload checks" by @konstin on 2026-01-09 11:50_

---

_Converted to draft by @konstin on 2026-01-09 12:05_

---

_Comment by @konstin on 2026-01-16 09:05_

I've verified these changes with testpypi: https://test.pypi.org/project/ty-konsti-test/#files

I'll leave merging to the person who makes the release.

---

_Marked ready for review by @konstin on 2026-01-16 09:05_

---
