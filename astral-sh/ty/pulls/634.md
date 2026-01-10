```yaml
number: 634
title: Bump version to 0.0.1-alpha.9
type: pull_request
state: merged
author: dhruvmanila
labels:
  - release
assignees: []
merged: true
base: main
head: dhruv/release
created_at: 2025-06-11T10:54:42Z
updated_at: 2025-06-11T11:27:37Z
url: https://github.com/astral-sh/ty/pull/634
synced_at: 2026-01-10T02:34:10Z
```

# Bump version to 0.0.1-alpha.9

---

_Pull request opened by @dhruvmanila on 2025-06-11 10:54_

_No description provided._

---

_Label `release` added by @dhruvmanila on 2025-06-11 10:54_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:34 on 2025-06-11 10:58_

these three could possibly go under the "server" section, since I believe users would have been most likely to encounter them if they hovered over one of these AST nodes in an IDE context

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:43 on 2025-06-11 10:58_

I don't think this bug ever appeared in a released version of ty, so we can probably omit the changelog entry for the bug being fixed!

```suggestion
```

---

_@AlexWaygood approved on 2025-06-11 10:59_

---

_@dhruvmanila reviewed on 2025-06-11 11:03_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:34 on 2025-06-11 11:03_

I'm not sure if it makes sense to put them under "server" but I'm also not sure if they belong in the changelog because these were panics in our testing files. I'm fine with removing them but thought to put them under "bugs" in case anyone was facing them.

---

_Merged by @dhruvmanila on 2025-06-11 11:05_

---

_Closed by @dhruvmanila on 2025-06-11 11:05_

---

_Branch deleted on 2025-06-11 11:05_

---

_@AlexWaygood reviewed on 2025-06-11 11:12_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:34 on 2025-06-11 11:12_

The panics were _uncovered_ by improving our test coverage, but they could easily have been triggered by a user hovering over exactly the wrong AST node in their IDE. Here's an example of exactly such a crash that was previously reported by a user: https://github.com/astral-sh/ty/issues/366#issuecomment-2909869832. It was fixed in https://github.com/astral-sh/ruff/pull/18321. So I think these definitely deserve to be listed in the changelog!

---

_@AlexWaygood reviewed on 2025-06-11 11:13_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:34 on 2025-06-11 11:13_

The user-facing impact right now is that users of the server are less likely to see random panics when hovering over AST nodes in their IDE, which is why I would have put them under "server". Obviously not a huge deal, though!

---

_@dhruvmanila reviewed on 2025-06-11 11:27_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:34 on 2025-06-11 11:27_

Oh, I see. Thanks for the context! I'll keep this in mind

---
