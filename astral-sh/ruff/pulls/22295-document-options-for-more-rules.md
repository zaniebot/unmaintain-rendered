```yaml
number: 22295
title: Document options for more rules
type: pull_request
state: merged
author: ntBre
labels:
  - documentation
assignees: []
merged: true
base: main
head: brent/more-option-docs
created_at: 2025-12-30T00:10:31Z
updated_at: 2025-12-30T13:44:14Z
url: https://github.com/astral-sh/ruff/pull/22295
synced_at: 2026-01-10T16:36:19Z
```

# Document options for more rules

---

_Pull request opened by @ntBre on 2025-12-30 00:10_

Summary
--

This is a follow up to #22198 documenting more rule options I found while going
through all of our rules.

The second commit renames the internal
`flake8_gettext::Settings::functions_names` field to `function_names` to match
the external configuration option. I guess this is technically breaking because
it's exposed to users via `--show-settings`, but I don't think we consider that
part of our stable API. I can definitely revert that if needed, though.

The other changes are just like #22198, adding new `## Options` sections to
rules to document the settings they use. I missed these in the previous PR
because they were used outside the rule implementations themselves. Most of
these settings are checked where the rules' implementation functions are called
instead.

Oh, the last commit also updates the removal date for `typing.ByteString`, which
got pushed back in the 3.14 release. I snuck that in today since I never opened
this PR last week.

I also fixed one reference link in RUF041.

Test Plan
--

Docs checks in CI


---

_Label `documentation` added by @ntBre on 2025-12-30 00:10_

---

_Comment by @astral-sh-bot[bot] on 2025-12-30 00:20_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Marked ready for review by @ntBre on 2025-12-30 00:27_

---

_Review requested from @AlexWaygood by @ntBre on 2025-12-30 00:27_

---

_Review requested from @MichaReiser by @ntBre on 2025-12-30 00:27_

---

_@MichaReiser approved on 2025-12-30 07:33_

Thank you

---

_Merged by @ntBre on 2025-12-30 13:44_

---

_Closed by @ntBre on 2025-12-30 13:44_

---

_Branch deleted on 2025-12-30 13:44_

---
