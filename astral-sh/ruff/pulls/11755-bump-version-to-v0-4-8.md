```yaml
number: 11755
title: Bump version to v0.4.8
type: pull_request
state: merged
author: dhruvmanila
labels:
  - release
assignees: []
merged: true
base: main
head: dhruv/bump
created_at: 2024-06-05T14:49:02Z
updated_at: 2024-06-05T15:33:11Z
url: https://github.com/astral-sh/ruff/pull/11755
synced_at: 2026-01-10T21:56:00Z
```

# Bump version to v0.4.8

---

_Pull request opened by @dhruvmanila on 2024-06-05 14:49_

_No description provided._

---

_Label `release` added by @dhruvmanila on 2024-06-05 14:49_

---

_@dhruvmanila reviewed on 2024-06-05 14:49_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:26 on 2024-06-05 14:49_

This was marked as "configuration" but I've changed it to "CLI" section

---

_@AlexWaygood reviewed on 2024-06-05 14:53_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:7 on 2024-06-05 14:53_

I would lead with the user-visible change, and then explain how it was achieved:

```suggestion
- Linter performance has been improved by 25% or more on some microbenchmarks
  by refactoring the lexer and parser to maintain synchronicity between them
  ([#11457](https://github.com/astral-sh/ruff/pull/11457))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:30 on 2024-06-05 14:54_

```suggestion
- \[`pyupgrade`\] Write empty string in lieu of panic when fixing `UP032` ([#11696](https://github.com/astral-sh/ruff/pull/11696))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:32 on 2024-06-05 14:55_

```suggestion
- Ensure the expression generator adds a newline before ``type`` statements ([#11720](https://github.com/astral-sh/ruff/pull/11720))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:33 on 2024-06-05 14:56_

```suggestion
- Fix ``ANN201`` panic by ensuring the lexer considers BOM for the start offset ([#11732](https://github.com/astral-sh/ruff/pull/11732))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:35 on 2024-06-05 14:57_

```suggestion
- Fix various panics in rules that attempt to parse type annotations ([#11740](https://github.com/astral-sh/ruff/pull/11740))
```

---

_@AlexWaygood reviewed on 2024-06-05 14:57_

Couple more suggestions to make the changelog entries focus on things users might be more interested in learning about:

---

_@dhruvmanila reviewed on 2024-06-05 15:05_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:33 on 2024-06-05 15:05_

I wonder if this should even be included because this bug doesn't really exist on any release version.

---

_@AlexWaygood reviewed on 2024-06-05 15:05_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:33 on 2024-06-05 15:05_

Hmm, probably not then

---

_@dhruvmanila reviewed on 2024-06-05 15:06_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:33 on 2024-06-05 15:06_

Going to remove this entry.

---

_@dhruvmanila reviewed on 2024-06-05 15:06_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:35 on 2024-06-05 15:06_

Going to remove this entry.

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:7 on 2024-06-05 15:08_

I don't think it's 25% xD

It's the lexer that's improved to around 25% while the linter is at around 10%

---

_@dhruvmanila reviewed on 2024-06-05 15:08_

---

_@dhruvmanila reviewed on 2024-06-05 15:09_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:7 on 2024-06-05 15:09_

But, I agree with this wording, thanks for the suggestion

---

_@AlexWaygood reviewed on 2024-06-05 15:09_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:7 on 2024-06-05 15:09_

Ah right, yeah :)

("Only" 10% 😆)

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-06-05 15:13_

---

_@AlexWaygood approved on 2024-06-05 15:14_

🚀

---

_Merged by @dhruvmanila on 2024-06-05 15:21_

---

_Closed by @dhruvmanila on 2024-06-05 15:21_

---

_Branch deleted on 2024-06-05 15:21_

---

_Comment by @github-actions[bot] on 2024-06-05 15:33_

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
