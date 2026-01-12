```yaml
number: 20998
title: "[`flake8-bugbear`] Skip `B905` and `B912` if <2 iterables and no starred arguments"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: main
head: zip-strict
created_at: 2025-10-20T16:01:27Z
updated_at: 2025-10-20T23:35:33Z
url: https://github.com/astral-sh/ruff/pull/20998
synced_at: 2026-01-12T15:57:14Z
```

# [`flake8-bugbear`] Skip `B905` and `B912` if <2 iterables and no starred arguments

---

_@dylwil3_

Closes #20997

This will _decrease_ the number of diagnostics emitted for [zip-without-explicit-strict (B905)](https://docs.astral.sh/ruff/rules/zip-without-explicit-strict/#zip-without-explicit-strict-b905), since previously it triggered on any `zip` call no matter the number of arguments. It may _increase_ the number of diagnostics for [map-without-explicit-strict (B912)](https://docs.astral.sh/ruff/rules/map-without-explicit-strict/#map-without-explicit-strict-b912) since it will now trigger on a single starred argument where before it would not. However, the latter rule is in `preview` so this is acceptable.

Note - we do not need to make any changes to [batched-without-explicit-strict (B911)](https://docs.astral.sh/ruff/rules/batched-without-explicit-strict/#batched-without-explicit-strict-b911) since that just takes a single iterable.

I am doing this in one PR rather than two because we should keep the behavior of these rules consistent with one another.

For review: apologies for the unreadability of the snapshot for `B905`. Unfortunately I saw no way of keeping a small diff and a correct fixture (the fixture labeled a whole block as `# Error` whereas now several in the block became `# Ok`).Probably simplest to just view the actual snapshot - it's relatively small.

---

_Label `rule` added by @dylwil3 on 2025-10-20 16:01_

---

_Renamed from "[`flake8-bugbear`] Skip `B905` and `B905` if <2 iterators and no starred arguments" to "[`flake8-bugbear`] Skip `B905` and `B905` if <2 iteratables and no starred arguments" by @dylwil3 on 2025-10-20 16:02_

---

_Comment by @github-actions[bot] on 2025-10-20 17:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[`flake8-bugbear`] Skip `B905` and `B905` if <2 iteratables and no starred arguments" to "[`flake8-bugbear`] Skip `B905` and `B905` if <2 iterables and no starred arguments" by @AlexWaygood on 2025-10-20 23:22_

---

_Renamed from "[`flake8-bugbear`] Skip `B905` and `B905` if <2 iterables and no starred arguments" to "[`flake8-bugbear`] Skip `B905` and `B912` if <2 iterables and no starred arguments" by @dylwil3 on 2025-10-20 23:34_

---

_Merged by @dylwil3 on 2025-10-20 23:35_

---

_Closed by @dylwil3 on 2025-10-20 23:35_

---
