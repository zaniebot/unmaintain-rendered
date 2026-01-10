```yaml
number: 13127
title: Distinguish between maybe and no side effects in contains_effects()
type: pull_request
state: closed
author: JonathanPlasse
labels: []
assignees: []
draft: true
base: main
head: maybe-contain-effects
created_at: 2024-08-27T20:36:36Z
updated_at: 2024-12-30T09:08:34Z
url: https://github.com/astral-sh/ruff/pull/13127
synced_at: 2026-01-10T20:42:26Z
```

# Distinguish between maybe and no side effects in contains_effects()

---

_Pull request opened by @JonathanPlasse on 2024-08-27 20:36_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This change introduce the `Maybe` to `contains_effects()` to be able to distinguish between possible and no side effects cases, and thus allowing a finer control of the behavior of the rule.

This was talked about in this comment issue as a way to address the issue.
- https://github.com/astral-sh/ruff/issues/12953#issuecomment-2295449654

The following list contains all rules using `contains_effects()`.
A description for each will be added to describe how the introduction of `Maybe` change the behavior of the rule in preview.

- [x] RUF019
  - The fix become unsafe when `contains_effects()` returns `Maybe::Maybe`
- [ ] B018
- [ ] SIM101
- [ ] SIM109
- [ ] SIM220
- [ ] SIM221
- [ ] SIM222
- [ ] SIM223
- [ ] SIM401
- [ ] SIM116
- [ ] SIM108
- [ ] F841
- [ ] PLR1714
- [ ] FURB132
- [ ] FURB110
- [ ] FURB105
- [ ] TRY300

## Test Plan

- [x] Add the problematic code from #12953 in RUF019 fixture
- [x] Add a preview snapshot of RUF019
- [x] Check there are no regression in the ecosystem for the stable version
<!-- How was it tested? -->


---

_Renamed from "Make contains" to "Distinguish between low possibility and no side effects in contains_effects()" by @JonathanPlasse on 2024-08-27 20:38_

---

_Renamed from "Distinguish between low possibility and no side effects in contains_effects()" to "Distinguish between maybe and no side effects in contains_effects()" by @JonathanPlasse on 2024-08-27 20:39_

---

_Comment by @github-actions[bot] on 2024-08-27 20:55_

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

_Comment by @JonathanPlasse on 2024-10-07 21:11_

Would it makes sense to wait for red knot to better detect side effects instead of duplicating work here?

---

_Comment by @MichaReiser on 2024-10-08 06:31_

> Would it makes sense to wait for red knot to better detect side effects instead of duplicating work here?

Hi @JonathanPlasse 

Red Knot is far out and we should not block Ruff improvements because of it.

---

_Comment by @JonathanPlasse on 2024-10-08 17:07_

Understood, will continue working on it.

---

_Marked ready for review by @JonathanPlasse on 2024-10-19 17:19_

---

_Converted to draft by @JonathanPlasse on 2024-10-20 10:12_

---

_Comment by @MichaReiser on 2024-12-30 09:08_

Thanks @JonathanPlasse for working on this. I'll close this PR because of inactivity. If someone's interested to pick up the work, feel free to submit a new PR based on it and reference this PR in your description

---

_Closed by @MichaReiser on 2024-12-30 09:08_

---
