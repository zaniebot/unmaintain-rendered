```yaml
number: 778
title: Bump version to 0.0.1-alpha.14
type: pull_request
state: merged
author: AlexWaygood
labels:
  - release
assignees: []
merged: true
base: main
head: release
created_at: 2025-07-08T10:39:41Z
updated_at: 2025-07-08T11:46:09Z
url: https://github.com/astral-sh/ty/pull/778
synced_at: 2026-01-10T02:34:10Z
```

# Bump version to 0.0.1-alpha.14

---

_Pull request opened by @AlexWaygood on 2025-07-08 10:39_

_No description provided._

---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-07-08 10:40_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-08 10:40_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:38 on 2025-07-08 10:46_

This is off by default and can be enabled by setting `ty.diagnosticMode` to `"workspace"`.

Oh wait, I need to add documentation for this config option!

---

_@dhruvmanila reviewed on 2025-07-08 10:46_

---

_@AlexWaygood reviewed on 2025-07-08 10:49_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:38 on 2025-07-08 10:49_

> Oh wait, I need to add documentation for this config option!

Should I hold off cutting the release until you've added that? Or are you okay if we document it after the release goes out?

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:38 on 2025-07-08 10:52_

https://github.com/astral-sh/ty/pull/779

---

_@dhruvmanila reviewed on 2025-07-08 10:52_

---

_@dhruvmanila reviewed on 2025-07-08 10:52_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:38 on 2025-07-08 10:52_

I think we'd need to add it before the release goes off because the docs are built on release, right?

---

_@AlexWaygood reviewed on 2025-07-08 10:54_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:38 on 2025-07-08 10:54_

I guess so! I don't think I'll need to rebase this branch after we land that PR, right? I _think_ it'll be okay if we just merge them both and then I set the workflow running

---

_@sharkdp reviewed on 2025-07-08 11:00_

---

_Review comment by @sharkdp on `CHANGELOG.md`:38 on 2025-07-08 11:00_

> I think we'd need to add it before the release goes off because the docs are built on release, right?

Correct.



> I guess so! I don't think I'll need to rebase this branch after we land that PR, right? I _think_ it'll be okay if we just merge them both and then I set the workflow running

Yes

---

_@sharkdp reviewed on 2025-07-08 11:02_

---

_Review comment by @sharkdp on `CHANGELOG.md`:40 on 2025-07-08 11:02_

Maybe just
```suggestion
- Use Python syntax highlighting in on-hover content ([#19082](https://github.com/astral-sh/ruff/pull/19082))
```

---

_@sharkdp reviewed on 2025-07-08 11:02_

---

_Review comment by @sharkdp on `CHANGELOG.md`:48 on 2025-07-08 11:02_

```suggestion
- Support declared-only instance attributes such as `self.x: int` ([#19048](https://github.com/astral-sh/ruff/pull/19048))
```

---

_Review comment by @sharkdp on `CHANGELOG.md`:42 on 2025-07-08 11:03_

We tried to avoid "Other changes" elsewhere. Should this be
```suggestion
### Typing semantics and features
```

---

_@sharkdp reviewed on 2025-07-08 11:03_

---

_@AlexWaygood reviewed on 2025-07-08 11:04_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:48 on 2025-07-08 11:04_

fixed it before you commented ;)

---

_@sharkdp reviewed on 2025-07-08 11:04_

---

_Review comment by @sharkdp on `CHANGELOG.md`:46 on 2025-07-08 11:04_

In my opinion, this deserves to be higher up. It's a fantastic new feature that removes many false positives.

---

_@sharkdp reviewed on 2025-07-08 11:04_

---

_Review comment by @sharkdp on `CHANGELOG.md`:36 on 2025-07-08 11:04_

Could these be inlined into a single paragraph?

---

_@AlexWaygood reviewed on 2025-07-08 11:05_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:42 on 2025-07-08 11:05_

I think that works well here!

---

_@sharkdp reviewed on 2025-07-08 11:05_

---

_Review comment by @sharkdp on `CHANGELOG.md`:22 on 2025-07-08 11:05_

Minor: Can we remove these empty lines in the list here? They have an effect on the formatting. If you did this intentionally for some reason, feel free to keep it of course.

---

_@sharkdp approved on 2025-07-08 11:06_

Thank you!

---

_@AlexWaygood reviewed on 2025-07-08 11:10_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:36 on 2025-07-08 11:10_

They could be; I agree that this entry feels like it's taking up a disproportionate amount of space right now. If it matters to a user which tokens are currently supported, though, then I think the bulleted list makes it easier to see at a glance if one you care about is listed... I'm not sure why a user would care _that_ much about that, though. @dhruvmanila, what do you think?

---

_@AlexWaygood reviewed on 2025-07-08 11:12_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:36 on 2025-07-08 11:12_

I think I'll just inline the list into a paragraph as you suggest, as it also allows me to fix your nit at https://github.com/astral-sh/ty/pull/778#discussion_r2192189863 :-)

---

_@AlexWaygood reviewed on 2025-07-08 11:19_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:46 on 2025-07-08 11:19_

moved it to be the top item in the "typing semantics" section

---

_Label `release` added by @AlexWaygood on 2025-07-08 11:21_

---

_Merged by @AlexWaygood on 2025-07-08 11:46_

---

_Closed by @AlexWaygood on 2025-07-08 11:46_

---

_Branch deleted on 2025-07-08 11:46_

---
