```yaml
number: 14862
title: Stop referring to early ruff versions
type: pull_request
state: merged
author: DimitriPapadopoulos
labels:
  - documentation
assignees: []
merged: true
base: main
head: v
created_at: 2024-12-09T08:38:10Z
updated_at: 2024-12-09T16:07:55Z
url: https://github.com/astral-sh/ruff/pull/14862
synced_at: 2026-01-10T20:42:27Z
```

# Stop referring to early ruff versions

---

_Pull request opened by @DimitriPapadopoulos on 2024-12-09 08:38_

## Summary

Referring to old versions has become more distracting than useful.

## Test Plan

—

---

_Review comment by @DimitriPapadopoulos on `docs/formatter.md`:23 on 2024-12-09 09:06_

Perhaps this shouldn't be a note any more, should it?

---

_Review comment by @DimitriPapadopoulos on `docs/linter.md`:22 on 2024-12-09 09:07_

Same.

---

_@DimitriPapadopoulos reviewed on 2024-12-09 09:07_

---

_Marked ready for review by @DimitriPapadopoulos on 2024-12-09 09:09_

---

_Review requested from @MichaReiser by @DimitriPapadopoulos on 2024-12-09 09:09_

---

_Review comment by @MichaReiser on `docs/formatter.md`:23 on 2024-12-09 09:13_

Yeah, I think we can remove the entire note here

---

_@MichaReiser reviewed on 2024-12-09 09:13_

---

_@MichaReiser reviewed on 2024-12-09 09:14_

---

_Review comment by @MichaReiser on `docs/linter.md`:22 on 2024-12-09 09:14_

agree

---

_@MichaReiser approved on 2024-12-09 09:15_

Thanks. This makes sense to me. I did a quick check and 0.1.3 is the version with the most downloads in the last 30 days (~90k) that is impacted by this documentation change. All other `<=0.1.7` versions have fewer than 50k downloads in the last 30 days. 



---

_Label `documentation` added by @MichaReiser on 2024-12-09 09:15_

---

_@dhruvmanila reviewed on 2024-12-09 09:16_

---

_Review comment by @dhruvmanila on `docs/tutorial.md`:353 on 2024-12-09 09:16_

We can add this file to https://github.com/astral-sh/ruff/blob/3d9ac535e92eac324776f94c8760b7220f45e624/pyproject.toml#L106-L113 so that it gets updated automatically on every release.

---

_@DimitriPapadopoulos reviewed on 2024-12-09 15:24_

---

_Review comment by @DimitriPapadopoulos on `docs/linter.md`:22 on 2024-12-09 15:24_

I have just removed the notes.

---

_@DimitriPapadopoulos reviewed on 2024-12-09 15:24_

---

_Review comment by @DimitriPapadopoulos on `docs/tutorial.md`:353 on 2024-12-09 15:24_

Added `docs/tutorial.md`.

---

_Comment by @github-actions[bot] on 2024-12-09 15:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @MichaReiser on 2024-12-09 15:47_

---

_Closed by @MichaReiser on 2024-12-09 15:47_

---

_Branch deleted on 2024-12-09 16:07_

---
