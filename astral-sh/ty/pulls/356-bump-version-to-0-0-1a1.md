```yaml
number: 356
title: Bump version to 0.0.1a1
type: pull_request
state: merged
author: MichaReiser
labels:
  - release
assignees: []
merged: true
base: main
head: micha/alpha-8
created_at: 2025-05-13T12:45:22Z
updated_at: 2025-05-13T15:40:02Z
url: https://github.com/astral-sh/ty/pull/356
synced_at: 2026-01-12T15:54:27Z
```

# Bump version to 0.0.1a1

---

_@MichaReiser_

_No description provided._

---

_Label `release` added by @MichaReiser on 2025-05-13 12:45_

---

_Marked ready for review by @MichaReiser on 2025-05-13 12:50_

---

_@sharkdp approved on 2025-05-13 12:52_

---

_Renamed from "Bump version to 0.0.1-alpha.1" to "Bump version to 0.0.1a1" by @MichaReiser on 2025-05-13 12:53_

---

_@AlexWaygood approved on 2025-05-13 13:02_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:7 on 2025-05-13 14:17_

```suggestion
- Fix infinite recursion bug in `is_disjoint_from` ([#18043](https://github.com/astral-sh/ruff/pull/18043))
```

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:32 on 2025-05-13 14:25_

I'd add a "Enhancements" category similar to uv which groups a lot of the changes that doesn't fall into the existing categories. We're still doing preview releases so there are going to be tons of features added.

Categorizing the other changes:
1. Enhancements / CLI
2. Enhancements / CLI
3. Enhancements
4. CLI
5. Enhancements
6. Enhancements

What do you think?

---

_@dhruvmanila approved on 2025-05-13 14:25_

---

_@dhruvmanila reviewed on 2025-05-13 14:26_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:32 on 2025-05-13 14:26_

(I can update `pyproject.toml` to inform rooster to categorize certain labels as "Enhancements" after the release.)

---

_Review comment by @MichaReiser on `CHANGELOG.md`:32 on 2025-05-13 14:27_

We can rename the section. I feel pretty neutral about it either way. I don't think 4 is CLI, because it is also supported for the `environment.python` configuration.

---

_@MichaReiser reviewed on 2025-05-13 14:27_

---

_@dhruvmanila reviewed on 2025-05-13 15:27_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:32 on 2025-05-13 15:27_

Renamed the section "Other changes" to "Enhancements" and moved it to the top.

---

_Merged by @dhruvmanila on 2025-05-13 15:40_

---

_Closed by @dhruvmanila on 2025-05-13 15:40_

---

_Branch deleted on 2025-05-13 15:40_

---
