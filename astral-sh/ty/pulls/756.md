```yaml
number: 756
title: Bump version to 0.0.1a13
type: pull_request
state: merged
author: sharkdp
labels: []
assignees: []
merged: true
base: main
head: david/bump-0-0-1a13
created_at: 2025-07-02T16:23:37Z
updated_at: 2025-07-02T17:39:06Z
url: https://github.com/astral-sh/ty/pull/756
synced_at: 2026-01-10T02:34:10Z
```

# Bump version to 0.0.1a13

---

_Pull request opened by @sharkdp on 2025-07-02 16:23_

_No description provided._

---

_@sharkdp reviewed on 2025-07-02 16:24_

---

_Review comment by @sharkdp on `CHANGELOG.md`:19 on 2025-07-02 16:24_

@AlexWaygood Do you have a more concise/user-facing description here? Or should we remove this?

---

_@zanieb reviewed on 2025-07-02 16:36_

---

_Review comment by @zanieb on `CHANGELOG.md`:33 on 2025-07-02 16:36_

We use a "Performance" section elsewhere.

---

_@zanieb reviewed on 2025-07-02 16:36_

---

_Review comment by @zanieb on `CHANGELOG.md`:38 on 2025-07-02 16:36_

We should probably note the documentation moved!

---

_@sharkdp reviewed on 2025-07-02 16:37_

---

_Review comment by @sharkdp on `CHANGELOG.md`:38 on 2025-07-02 16:37_

Thought about it, but I think Micha told me that documentation changes are usually not included in the CHANGELOG, so I removed them. This does seem worth mentioning, though.

---

_@AlexWaygood reviewed on 2025-07-02 16:39_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:19 on 2025-07-02 16:39_

Could add a sentence along the lines of "This reduces the chance that type narrowing from a `hasattr()` check would lead to ty inferring `Never`, among other things.?

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:7 on 2025-07-02 16:39_

```suggestion
- Fix stack overflows related to mutually recursive protocols ([#19003](https://github.com/astral-sh/ruff/pull/19003))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:22 on 2025-07-02 16:40_

```suggestion
- Improve type-inference for `__import__(name)` and `importlib.import_module(name)` ([#19008](https://github.com/astral-sh/ruff/pull/19008))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:25 on 2025-07-02 16:41_

```suggestion
- Eagerly evaluate certain constraints when analyzing control flow ([#18998](https://github.com/astral-sh/ruff/pull/18998), [#19044](https://github.com/astral-sh/ruff/pull/19044), [#19068](https://github.com/astral-sh/ruff/pull/19068))
```

---

_@AlexWaygood approved on 2025-07-02 16:42_

---

_Merged by @sharkdp on 2025-07-02 17:39_

---

_Closed by @sharkdp on 2025-07-02 17:39_

---

_Branch deleted on 2025-07-02 17:39_

---
