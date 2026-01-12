```yaml
number: 9231
title: Bump version to v0.1.9
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/bump
created_at: 2023-12-21T17:33:16Z
updated_at: 2023-12-21T19:01:11Z
url: https://github.com/astral-sh/ruff/pull/9231
synced_at: 2026-01-12T15:55:28Z
```

# Bump version to v0.1.9

---

_@BurntSushi_

_No description provided._

---

_Review requested from @charliermarsh by @BurntSushi on 2023-12-21 17:33_

---

_Review requested from @zanieb by @BurntSushi on 2023-12-21 17:33_

---

_Review comment by @zanieb on `CHANGELOG.md`:62 on 2023-12-21 17:35_

We can drop all these dependabot changes, we should be excluding these by default... I wonder if something's up with the labeling

---

_@zanieb reviewed on 2023-12-21 17:35_

---

_@zanieb reviewed on 2023-12-21 17:36_

---

_Review comment by @zanieb on `CHANGELOG.md`:60 on 2023-12-21 17:36_

Similarly we can drop internal changes related to CI, we should add the `ci` label to the excluded set in our rooster settings in the `pyproject.toml`

---

_@zanieb reviewed on 2023-12-21 17:36_

---

_Review comment by @zanieb on `CHANGELOG.md`:54 on 2023-12-21 17:36_

This (and similar) should be moved into rule changes

---

_Review comment by @zanieb on `CHANGELOG.md`:52 on 2023-12-21 17:38_

This is also `internal` labeled — we should drop it. I'm confused since this should be excluded by default — I can look into the issue later.

---

_@zanieb reviewed on 2023-12-21 17:38_

---

_@zanieb reviewed on 2023-12-21 17:38_

---

_Review comment by @zanieb on `CHANGELOG.md`:59 on 2023-12-21 17:38_

Rule change? Might want to reference the rule code

---

_Review comment by @zanieb on `CHANGELOG.md`:65 on 2023-12-21 17:38_

Rule change

---

_@zanieb reviewed on 2023-12-21 17:38_

---

_@zanieb reviewed on 2023-12-21 17:39_

---

_Review comment by @zanieb on `CHANGELOG.md`:27 on 2023-12-21 17:39_

Was this for `--check`? I'd make it reference the user facing change e.g.

```suggestion
- Update `ruff format --check` to display message for already formatted files ([#9153](https://github.com/astral-sh/ruff/pull/9153))
```

---

_@zanieb reviewed on 2023-12-21 17:39_

---

_Review comment by @zanieb on `CHANGELOG.md`:27 on 2023-12-21 17:39_

(I did not check the PR here)

---

_@zanieb reviewed on 2023-12-21 17:40_

---

_Review comment by @zanieb on `CHANGELOG.md`:23 on 2023-12-21 17:40_

Internal, can drop

---

_Review comment by @zanieb on `CHANGELOG.md`:32 on 2023-12-21 17:41_

Not critical, but I try to briefly edit these for consistency
```suggestion
- Fix panic in `D208` with multibyte indent ([#9147](https://github.com/astral-sh/ruff/pull/9147))
```

---

_@zanieb reviewed on 2023-12-21 17:41_

---

_Comment by @github-actions[bot] on 2023-12-21 17:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@BurntSushi reviewed on 2023-12-21 17:46_

---

_Review comment by @BurntSushi on `CHANGELOG.md`:54 on 2023-12-21 17:46_

Do you see any others that should be moved to rule changes? I went through a few that might look it, but ultimately they seemed better as bug fixes.

---

_@zanieb approved on 2023-12-21 18:11_

lgtm!

---

_@charliermarsh approved on 2023-12-21 18:19_

---

_Merged by @BurntSushi on 2023-12-21 18:19_

---

_Closed by @BurntSushi on 2023-12-21 18:19_

---

_Branch deleted on 2023-12-21 18:19_

---

_Comment by @zanieb on 2023-12-21 19:01_

Fixed the ignored labels in https://github.com/zanieb/rooster/pull/12

---
