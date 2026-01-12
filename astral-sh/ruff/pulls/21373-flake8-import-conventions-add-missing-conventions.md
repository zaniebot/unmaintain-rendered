```yaml
number: 21373
title: "[`flake8-import-conventions`] Add missing conventions from upstream (`ICN001`, `ICN002`)"
type: pull_request
state: open
author: danparizher
labels:
  - rule
  - preview
assignees: []
base: main
head: fix-21300
created_at: 2025-11-11T04:25:41Z
updated_at: 2025-12-03T20:10:43Z
url: https://github.com/astral-sh/ruff/pull/21373
synced_at: 2026-01-12T15:57:22Z
```

# [`flake8-import-conventions`] Add missing conventions from upstream (`ICN001`, `ICN002`)

---

_@danparizher_

## Summary

Adds missing import conventions from upstream flake8-import-conventions plugin. Specifically:
- `ICN001`: Enforces `plotly.graph_objects` → `go` and `statsmodels.api` → `sm`
- `ICN002`: Bans `geopandas` → `gpd` alias

Fixes #21300

## Problem Analysis

Ruff's `ICN001` (unconventional-import-alias) and `ICN002` (banned-import-alias) rules were missing some conventions enforced by the upstream [flake8-import-conventions](https://github.com/joaopalmeiro/flake8-import-conventions) plugin:

1. **ICN001 missing conventions:**
   - `plotly.graph_objects` should be imported as `go` (IC008 in upstream)
   - `statsmodels.api` should be imported as `sm` (IC010 in upstream)

2. **ICN002 missing convention:**
   - `geopandas` should not be imported as `gpd` (IC002 in upstream)

The root cause was that these conventions were not included in Ruff's default configuration. The `CONVENTIONAL_ALIASES` constant was missing the two ICN001 entries, and there was no default banned aliases configuration for ICN002.

## Approach

1. **Added missing ICN001 conventions:**
   - Added `("plotly.graph_objects", "go")` and `("statsmodels.api", "sm")` to `CONVENTIONAL_ALIASES` in `settings.rs`
   - Updated the default option string in `options.rs` to include these new aliases

2. **Added default banned aliases for ICN002:**
   - Created `default_banned_aliases()` function returning `geopandas` → `["gpd"]`
   - Updated `Settings::default()` to use `default_banned_aliases()`
   - Updated `try_into_settings()` in `options.rs` to use `default_banned_aliases()` when `banned_aliases` is None
   - Updated the default option string for `banned_aliases` in `options.rs`

3. **Added comprehensive tests:**
   - Created `missing_conventions.py` test fixture with cases for all three new conventions
   - Added `missing_conventions` test in `mod.rs` to verify the new conventions work correctly

## Test Plan

Added new snapshot test `missing_conventions` that verifies:
- ICN001 correctly flags `plotly.graph_objects` without alias and requires `go`
- ICN001 correctly flags `statsmodels.api` without alias and requires `sm`
- ICN002 correctly flags `geopandas as gpd` as banned
- All existing tests continue to pass (10/10 tests passing)

The fix has been manually verified to match the behavior of upstream flake8-import-conventions.

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 04:33_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/d44186034851e0660af6342f319d00296cded660/ibis/backends/bigquery/converter.py#L9'>ibis/backends/bigquery/converter.py:9:9:</a> ICN002 `geopandas` should not be imported as `gpd`
+ <a href='https://github.com/ibis-project/ibis/blob/d44186034851e0660af6342f319d00296cded660/ibis/backends/postgres/converter.py#L11'>ibis/backends/postgres/converter.py:11:9:</a> ICN002 `geopandas` should not be imported as `gpd`
+ <a href='https://github.com/ibis-project/ibis/blob/d44186034851e0660af6342f319d00296cded660/ibis/expr/api.py#L52'>ibis/expr/api.py:52:5:</a> ICN002 `geopandas` should not be imported as `gpd`
+ <a href='https://github.com/ibis-project/ibis/blob/d44186034851e0660af6342f319d00296cded660/ibis/formats/pandas.py#L174'>ibis/formats/pandas.py:174:9:</a> ICN002 `geopandas` should not be imported as `gpd`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ICN002 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>





---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_import_conventions/missing_conventions.py`:1 on 2025-11-11 13:29_

Should we just add these to one of the existing test files? `defaults.py` seems promising. `missing_conventions` seems a bit misleading since they're not missing anymore after your PR.

---

_@ntBre requested changes on 2025-11-11 13:34_

Thanks! This looks good, just one suggestion about where to put the new tests

---

_Label `rule` added by @ntBre on 2025-11-11 13:34_

---

_Review requested from @ntBre by @danparizher on 2025-11-11 22:20_

---

_@ntBre reviewed on 2025-11-13 22:38_

On second thought, could we make this a preview change? Initially I assumed this was an unintentional omission from the original PR adding the rules (https://github.com/astral-sh/ruff/pull/1098), but I see now that these were changed upstream _after_ our rule was added (https://github.com/joaopalmeiro/flake8-import-conventions/issues/3, https://github.com/joaopalmeiro/flake8-import-conventions/issues/4). These issues were opened upstream before our rule but closed after. And there are a few hits in the ecosystem report for `geopandas` too.

---

_Review requested from @ntBre by @danparizher on 2025-11-14 02:19_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_import_conventions/rules/banned_import_alias.rs`:70 on 2025-11-18 19:46_

Do we need to change this code? I thought we could handle this just by adjusting the preview behavior of the default settings.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_import_conventions/settings.rs`:96 on 2025-11-18 19:53_

I think the way we usually handle this is by defining a method that takes a `preview` argument instead of `impl Default` (which can't take arguments). This is kind of the opposite since it was a stabilization PR, but you can see an example here: https://github.com/astral-sh/ruff/pull/12838/files#diff-0e4daa1a461406d9fd7af87b85ff4ecc89bd61f748d2b7cb24b88b25c7d5ba59

Possibly a better example: https://github.com/astral-sh/ruff/pull/15951

---

_@ntBre reviewed on 2025-11-18 19:58_

---

_Review requested from @ntBre by @danparizher on 2025-11-19 23:31_

---

_Label `preview` added by @ntBre on 2025-12-03 19:44_

---

_Review comment by @ntBre on `crates/ruff_workspace/src/options.rs`:1680 on 2025-12-03 20:08_

I don't think we should set this here. If I understand correctly, this will extend the list of aliases when preview is enabled even if the user provided a set of aliases. We only want to extend the _default_ set in the case where `self.aliases` is `None` above. I think it would make sense for the `default_aliases` helper function to take the `preview` argument and apply it there.

---

_Review comment by @ntBre on `crates/ruff_workspace/src/options.rs`:1697 on 2025-12-03 20:08_

Same as above, we should only do this in the `or_default` case.

---

_@ntBre reviewed on 2025-12-03 20:10_

Thanks, this is looking good. I fixed a few nits, reverting some unrelated changes and tweaking the test setup, but I also wanted to get your thoughts on the `try_into_settings` changes. See my inline comments for that.

---
