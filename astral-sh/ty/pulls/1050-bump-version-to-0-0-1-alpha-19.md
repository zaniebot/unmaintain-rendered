```yaml
number: 1050
title: Bump version to 0.0.1-alpha.19
type: pull_request
state: merged
author: AlexWaygood
labels:
  - release
assignees: []
merged: true
base: main
head: alex/release
created_at: 2025-08-19T12:58:56Z
updated_at: 2025-08-19T13:14:41Z
url: https://github.com/astral-sh/ty/pull/1050
synced_at: 2026-01-10T02:34:10Z
```

# Bump version to 0.0.1-alpha.19

---

_Pull request opened by @AlexWaygood on 2025-08-19 12:58_

_No description provided._

---

_Label `release` added by @AlexWaygood on 2025-08-19 12:58_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:13 on 2025-08-19 12:59_

I hope this is a good description of the user-facing impact of https://github.com/astral-sh/ruff/pull/19908

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:16 on 2025-08-19 12:59_

quite a few of these this release, so I put them in their own section

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:29 on 2025-08-19 13:00_

I hope this is a good description of the user-facing impact of https://github.com/astral-sh/ruff/pull/19786 !

---

_@AlexWaygood reviewed on 2025-08-19 13:00_

---

_@dhruvmanila reviewed on 2025-08-19 13:04_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:16 on 2025-08-19 13:04_

nit: we could rename "Other semantics improvements" to (the existing) "Typing semantics and features" and have this section as a sub-section under it?

---

_@sharkdp reviewed on 2025-08-19 13:05_

---

_Review comment by @sharkdp on `CHANGELOG.md`:8 on 2025-08-19 13:05_

```suggestion
- Fix ANSI colors in terminal output on old Windows terminals ([#19984](https://github.com/astral-sh/ruff/pull/19984))
```

---

_@dhruvmanila reviewed on 2025-08-19 13:06_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:30 on 2025-08-19 13:06_

Should this be just "kw_only" (remove "=True") ?

---

_@dhruvmanila reviewed on 2025-08-19 13:07_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:35 on 2025-08-19 13:07_

this could be removed?

---

_@sharkdp reviewed on 2025-08-19 13:07_

---

_Review comment by @sharkdp on `CHANGELOG.md`:40 on 2025-08-19 13:07_

(?)
```suggestion
- Short-circuit a server inlay hints request if it's disabled in settings ([#19963](https://github.com/astral-sh/ruff/pull/19963))
```

---

_@sharkdp approved on 2025-08-19 13:07_

Thanks!


---

_@AlexWaygood reviewed on 2025-08-19 13:08_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:30 on 2025-08-19 13:08_

```suggestion
- Support the `kw_only` parameter for `dataclasses.dataclass()` and `dataclasses.field()` ([#19677](https://github.com/astral-sh/ruff/pull/19677))
```

---

_@AlexWaygood reviewed on 2025-08-19 13:08_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:35 on 2025-08-19 13:08_

```suggestion
```

---

_@dhruvmanila reviewed on 2025-08-19 13:08_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:40 on 2025-08-19 13:08_

What about something like the following?
```suggestion
- Short-circuit a server inlay hints request if all settings under `ty.inlayHints` are disabled ([#19963](https://github.com/astral-sh/ruff/pull/19963))
```

---

_@AlexWaygood reviewed on 2025-08-19 13:09_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:40 on 2025-08-19 13:09_

oops, I applied @sharkdp's suggestion too quickly 😆

---

_@dhruvmanila reviewed on 2025-08-19 13:09_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:40 on 2025-08-19 13:09_

jinx 😆 

---

_@dhruvmanila approved on 2025-08-19 13:09_

---

_@AlexWaygood reviewed on 2025-08-19 13:10_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:16 on 2025-08-19 13:10_

Hmm, but then we'd have to have two subsections underneath the "Typing semantics and features" section, I think? No strong opinion but I weakly think I'd find it clearer as-is

---

_@AlexWaygood reviewed on 2025-08-19 13:12_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:40 on 2025-08-19 13:12_

applied this suggestion!

---

_Merged by @AlexWaygood on 2025-08-19 13:14_

---

_Closed by @AlexWaygood on 2025-08-19 13:14_

---

_Branch deleted on 2025-08-19 13:14_

---
