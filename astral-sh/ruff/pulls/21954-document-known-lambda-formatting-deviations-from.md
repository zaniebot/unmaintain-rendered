```yaml
number: 21954
title: Document known lambda formatting deviations from Black
type: pull_request
state: merged
author: ntBre
labels:
  - documentation
assignees: []
merged: true
base: main
head: brent/lambda-deviation
created_at: 2025-12-12T17:26:10Z
updated_at: 2025-12-12T17:57:11Z
url: https://github.com/astral-sh/ruff/pull/21954
synced_at: 2026-01-12T15:57:37Z
```

# Document known lambda formatting deviations from Black

---

_@ntBre_

Summary
--

Following #8179, we now format long lambda expressions a bit more like Black, preferring to keep long parameter lists on a single line, but we go one step further to break the body itself across multiple lines and parenthesize it if it's still too long. This PR documents both the stable deviation that breaks parameters across multiple lines, and the new preview deviation that breaks the body instead.

I also fixed a couple of typos in the section immediately above my addition.

Test Plan
--

I tested all of the snippets here against `main` for the preview behavior, our playground for the stable behavior, and Black's playground for their behavior


---

_Review requested from @MichaReiser by @ntBre on 2025-12-12 17:26_

---

_Label `documentation` added by @ntBre on 2025-12-12 17:26_

---

_@MichaReiser approved on 2025-12-12 17:53_

---

_Merged by @ntBre on 2025-12-12 17:57_

---

_Closed by @ntBre on 2025-12-12 17:57_

---

_Branch deleted on 2025-12-12 17:57_

---
