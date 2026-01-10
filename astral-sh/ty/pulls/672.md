```yaml
number: 672
title: Bump version to 0.0.1a11
type: pull_request
state: merged
author: sharkdp
labels: []
assignees: []
merged: true
base: main
head: david/bump-0-0-1a11
created_at: 2025-06-17T19:48:55Z
updated_at: 2025-06-17T20:28:09Z
url: https://github.com/astral-sh/ty/pull/672
synced_at: 2026-01-10T02:34:10Z
```

# Bump version to 0.0.1a11

---

_Pull request opened by @sharkdp on 2025-06-17 19:48_

_No description provided._

---

_Review comment by @carljm on `CHANGELOG.md`:22 on 2025-06-17 20:04_

```suggestion
- Improve reachability analysis ([#18621](https://github.com/astral-sh/ruff/pull/18621))
```

---

_Review comment by @carljm on `CHANGELOG.md`:7 on 2025-06-17 20:06_

```suggestion
- Stabilize auto-complete; remove the opt-in experimental setting ([#18650](https://github.com/astral-sh/ruff/pull/18650))
```

---

_@carljm approved on 2025-06-17 20:07_

Looks good! A couple suggested changes to stay consistent in the (grammatical) mood and tense of changelog entries (they are generally in the imperative mood.)

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:24 on 2025-06-17 20:14_

```suggestion
- Update typeshed stubs ([#18679](https://github.com/astral-sh/ruff/pull/18679)): [typeshed diff](https://github.com/python/typeshed/compare/5a3c495d2f6fa9b68cd99f39feba4426e4d17ea9...ecd5141cc036366cc9e3ca371096d6a14b0ccd13)
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:12 on 2025-06-17 20:16_

might be good to focus on the user-facing impact of this change:

```suggestion
- Fix panic that could occur when printing a class's "header" in diagnostic messages ([#18670](https://github.com/astral-sh/ruff/pull/18670))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:14 on 2025-06-17 20:16_

```suggestion
- Fix panics when "pulling types" for various special forms that have the wrong number of parameters. These could cause issues when hovering over symbols in an IDE. ([#18642](https://github.com/astral-sh/ruff/pull/18642))
```

---

_@AlexWaygood approved on 2025-06-17 20:17_

---

_Merged by @sharkdp on 2025-06-17 20:28_

---

_Closed by @sharkdp on 2025-06-17 20:28_

---

_Branch deleted on 2025-06-17 20:28_

---
