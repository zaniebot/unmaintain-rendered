```yaml
number: 22097
title: "[ty] List rules in alphabetical order in the reference docs"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: alex/alphabetise-docs
created_at: 2025-12-19T18:55:46Z
updated_at: 2025-12-19T19:11:07Z
url: https://github.com/astral-sh/ruff/pull/22097
synced_at: 2026-01-12T15:57:41Z
```

# [ty] List rules in alphabetical order in the reference docs

---

_@AlexWaygood_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1885.

It wasn't obvious to me that there was a deliberate order to the way these rules were listed in our reference docs -- it looked like it was _nearly_ alphabetical, but not quite. I think it's simpler if we just list them in alphabetical order.

## Test Plan

The output from running `cargo dev generate-all` (committed as part of this PR) looks correct!


---

_Review requested from @carljm by @AlexWaygood on 2025-12-19 18:55_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-12-19 18:55_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-19 18:55_

---

_Label `documentation` added by @AlexWaygood on 2025-12-19 18:55_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-19 18:55_

---

_Label `ty` added by @AlexWaygood on 2025-12-19 18:55_

---

_@charliermarsh approved on 2025-12-19 19:08_

---

_Merged by @AlexWaygood on 2025-12-19 19:11_

---

_Closed by @AlexWaygood on 2025-12-19 19:11_

---

_Branch deleted on 2025-12-19 19:11_

---
