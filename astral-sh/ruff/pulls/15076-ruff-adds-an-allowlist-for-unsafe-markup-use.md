```yaml
number: 15076
title: "[`ruff`] Adds an allowlist for `unsafe-markup-use` (`RUF035`)"
type: pull_request
state: merged
author: Daverball
labels:
  - configuration
assignees: []
merged: true
base: main
head: feat/markup-whitelist-calls
created_at: 2024-12-20T08:25:21Z
updated_at: 2024-12-20T09:37:41Z
url: https://github.com/astral-sh/ruff/pull/15076
synced_at: 2026-01-10T20:42:27Z
```

# [`ruff`] Adds an allowlist for `unsafe-markup-use` (`RUF035`)

---

_Pull request opened by @Daverball on 2024-12-20 08:25_

Closes: #14523

## Summary

Adds a whitelist of calls allowed to be used within a `markupsafe.Markup` call.

## Test Plan

`cargo nextest run`


---

_Review comment by @Daverball on `crates/ruff_linter/resources/test/fixtures/ruff/RUF035_whitelisted_markup_calls.py`:9 on 2024-12-20 08:28_

I considered making this work. But then we should probably also make the same thing work with string/bytes literals. At which point I would have to change some of the examples, so they still make sense. So I held off for now.

---

_@Daverball reviewed on 2024-12-20 08:28_

---

_Comment by @github-actions[bot] on 2024-12-20 08:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2024-12-20 08:40_

---

_Review comment by @dhruvmanila on `crates/ruff_workspace/src/options.rs`:3128 on 2024-12-20 08:40_

Should we use `allowed_markup_calls` instead to be consistent with other settings name?

---

_Label `configuration` added by @dhruvmanila on 2024-12-20 08:41_

---

_Renamed from "[`ruff`] Adds a whitelist for `unsafe-markup-use`." to "[`ruff`] Adds an allowlist for `unsafe-markup-use`." by @MichaReiser on 2024-12-20 08:46_

---

_@dhruvmanila reviewed on 2024-12-20 09:06_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/ruff/RUF035_whitelisted_markup_calls.py`:9 on 2024-12-20 09:06_

I think it's fine for now to limit this to only direct calls.

---

_@dhruvmanila reviewed on 2024-12-20 09:11_

---

_Review comment by @dhruvmanila on `crates/ruff_workspace/src/options.rs`:3122 on 2024-12-20 09:11_

I think these will need to be surrounded by backticks as the references are otherwise it won't be recognized (I think).

---

_@Daverball reviewed on 2024-12-20 09:17_

---

_Review comment by @Daverball on `crates/ruff_workspace/src/options.rs`:3122 on 2024-12-20 09:17_

Looks like you're right, this is already broken in the other setting, which has the same mistake: https://docs.astral.sh/ruff/settings/#lint_ruff_extend-markup-names

---

_@dhruvmanila reviewed on 2024-12-20 09:17_

---

_Review comment by @dhruvmanila on `crates/ruff_workspace/src/options.rs`:3122 on 2024-12-20 09:17_

Ok, adding backticks doesn't work (not sure why), you might need to inline the links via `[hello](link)` method.

---

_@Daverball reviewed on 2024-12-20 09:23_

---

_Review comment by @Daverball on `crates/ruff_workspace/src/options.rs`:3122 on 2024-12-20 09:23_

Interesting that none of the CI docs jobs catch this. Seems like an easy mistake to detect statically, but maybe there's something subtle at play, which would result in false positives in other cases.

---

_@dhruvmanila reviewed on 2024-12-20 09:27_

---

_Review comment by @dhruvmanila on `crates/ruff_workspace/src/options.rs`:3122 on 2024-12-20 09:27_

It might be something related to mkdocs-material rendering which doesn't allow this, you might need to inline the links.

---

_@dhruvmanila approved on 2024-12-20 09:28_

---

_Renamed from "[`ruff`] Adds an allowlist for `unsafe-markup-use`." to "[`ruff`] Adds an allowlist for `unsafe-markup-use` (`RUF035`)" by @dhruvmanila on 2024-12-20 09:28_

---

_@Daverball reviewed on 2024-12-20 09:28_

---

_Review comment by @Daverball on `crates/ruff_workspace/src/options.rs`:3122 on 2024-12-20 09:28_

Right, I think the docs for `options.rs` are a bit special, so a lot of the regular docs stuff doesn't work. You also can't reference other rules by name and instead have to manually link the anchor.

The reference works fine with or without backticks in the rule.

---

_Merged by @dhruvmanila on 2024-12-20 09:36_

---

_Closed by @dhruvmanila on 2024-12-20 09:36_

---

_Branch deleted on 2024-12-20 09:36_

---
