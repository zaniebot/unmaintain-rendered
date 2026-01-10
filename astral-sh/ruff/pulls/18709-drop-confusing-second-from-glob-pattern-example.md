```yaml
number: 18709
title: "Drop confusing second `*` from glob pattern example"
type: pull_request
state: merged
author: ntBre
labels:
  - documentation
assignees: []
merged: true
base: main
head: brent/fix-per-file-target-version-glob
created_at: 2025-06-16T14:09:11Z
updated_at: 2025-06-16T14:41:44Z
url: https://github.com/astral-sh/ruff/pull/18709
synced_at: 2026-01-10T18:45:04Z
```

# Drop confusing second `*` from glob pattern example

---

_Pull request opened by @ntBre on 2025-06-16 14:09_

Summary
--

As @AlexWaygood noted on the 0.12 release blog post draft, the existing example is a bit confusing. Either `**/*.py` or just `*.py`, as I went with here, makes more sense, although the old version (`scripts/**.py`) also worked when I tested it. However, this probably shouldn't be relied upon since the [globset](https://docs.rs/globset/latest/globset/#syntax) docs say:

> Using ** anywhere else is illegal

where "anywhere else" comes after the listing of the three valid positions:
1. At the start of a pattern (`**/`)
2. At the end of a pattern (`/**`)
3. Or directly between two slashes (`/**/`)

I think the current version is luckily treated the same as a single `*`, and the default globbing settings allow it to match subdirectories such that the new example pattern will apply to the whole `scripts` tree in a project like this:

```
.
├── README.md
├── pyproject.toml
├── scripts
│   ├── matching.py
│   └── sub
│       └── nested.py
└── src
    └── main.py
```

Test Plan
--

Local testing of the new pattern, but the specifics of the pattern aren't as important as having a more intuitive-looking/correct example.

---

_Label `documentation` added by @ntBre on 2025-06-16 14:09_

---

_Review requested from @MichaReiser by @ntBre on 2025-06-16 14:09_

---

_Comment by @github-actions[bot] on 2025-06-16 14:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-06-16 14:38_

---

_Merged by @ntBre on 2025-06-16 14:41_

---

_Closed by @ntBre on 2025-06-16 14:41_

---

_Branch deleted on 2025-06-16 14:41_

---
