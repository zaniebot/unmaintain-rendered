```yaml
number: 1869
title: update implicit root docs
type: pull_request
state: merged
author: Gankra
labels: []
assignees: []
merged: true
base: main
head: gankra/rootdoc
created_at: 2025-12-12T18:18:07Z
updated_at: 2025-12-13T20:17:12Z
url: https://github.com/astral-sh/ty/pull/1869
synced_at: 2026-01-12T15:54:28Z
```

# update implicit root docs

---

_@Gankra_

As of https://github.com/astral-sh/ruff/pull/21817, ./tests are no longer implicitly included.

---

_@AlexWaygood had review dismissed on 2025-12-12 18:27_

`docs/reference/configuration.md` is generated and needs to be updated over in the Ruff repo (see failing CI)

---

_Closed by @carljm on 2025-12-12 21:04_

---

_Comment by @carljm on 2025-12-12 21:04_

Oh sorry, didn't realize this also changes a non-generated file.

---

_Reopened by @carljm on 2025-12-12 21:04_

---

_Review comment by @carljm on `docs/modules.md`:31 on 2025-12-13 04:32_

Why do we discuss this down here, separately from the mention of `./src` above? Isn't the treatment of `./python` the same as the treatment of `./src`? (I realize that choice predates this PR.)

---

_Review comment by @carljm on `docs/modules.md`:8 on 2025-12-13 04:33_

```suggestion
By default, ty searches for first-party modules in the project's root directory and in `src` and `python`
subdirectories, if the latter are present and are not packages (don't contain an `__init__.py` or `__init__.pyi` file).
```

---

_@carljm approved on 2025-12-13 04:34_

---

_@Gankra reviewed on 2025-12-13 19:49_

---

_Review comment by @Gankra on `docs/modules.md`:31 on 2025-12-13 19:49_

Hmm other variants of this doc seemed to suggest there were two mutually exclusive "layout modes" we detected, but the ./python addition was applicable to both.

---

_Merged by @Gankra on 2025-12-13 19:53_

---

_Closed by @Gankra on 2025-12-13 19:53_

---

_Branch deleted on 2025-12-13 19:53_

---

_@carljm reviewed on 2025-12-13 19:56_

---

_Review comment by @carljm on `docs/modules.md`:31 on 2025-12-13 19:56_

I don't think that is the case...

---

_Comment by @Gankra on 2025-12-13 20:16_

Oh ha, I had auto-merge set from before Carl had comments, and Alex dismissing his review unblocked it, whoops!

---
