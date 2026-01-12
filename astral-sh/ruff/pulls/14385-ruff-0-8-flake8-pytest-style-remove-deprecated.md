```yaml
number: 14385
title: "[ruff 0.8] [`flake8-pytest-style`] Remove deprecated rules PT004 and PT005"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - breaking
assignees: []
merged: true
base: ruff-0.8
head: alex/remove-pt-rules
created_at: 2024-11-16T19:32:25Z
updated_at: 2024-11-19T09:01:36Z
url: https://github.com/astral-sh/ruff/pull/14385
synced_at: 2026-01-12T15:55:47Z
```

# [ruff 0.8] [`flake8-pytest-style`] Remove deprecated rules PT004 and PT005

---

_@AlexWaygood_

## Summary

Remove PT004 and PT005. These were deprecated in https://github.com/astral-sh/ruff/pull/12837, which landed as part of Ruff 0.6 (released on August 15th). There are no open issues on the tracker asking us to undeprecate them.

## Test Plan

- `cargo test`
- `git grep PytestMissingFixtureNameUnderscore`, `git grep PytestIncorrectFixtureNameUnderscore`, `git grep PT004` and `git grep PT005` all show no results
- Wait to see what the ecosystem report looks like


---

_Label `breaking` added by @AlexWaygood on 2024-11-16 19:32_

---

_Renamed from "[ruff 0.8] Remove deprecated rules PT004 and PT005" to "[ruff 0.8] [`flake8-pytest-style`] Remove deprecated rules PT004 and PT005" by @AlexWaygood on 2024-11-16 19:37_

---

_Comment by @github-actions[bot] on 2024-11-16 19:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -3 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/4c9062b078d11c99c85923f93872cffc37548f27/molecule/testinfra/ossec/test_journalist_mail.py#L14'>molecule/testinfra/ossec/test_journalist_mail.py:14:45:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PT004`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/e583793a16ebe806b1fffcea1e2689d754f6425d/rotkehlchen/tests/fixtures/globaldb.py#L207'>rotkehlchen/tests/fixtures/globaldb.py:207:5:</a> PT004 Fixture `fixture_historical_price_test_data` does not return anything, add leading underscore
- <a href='https://github.com/rotki/rotki/blob/e583793a16ebe806b1fffcea1e2689d754f6425d/rotkehlchen/tests/fixtures/oracles.py#L20'>rotkehlchen/tests/fixtures/oracles.py:20:5:</a> PT004 Fixture `fixture_cache_coinlist` does not return anything, add leading underscore
- <a href='https://github.com/rotki/rotki/blob/e583793a16ebe806b1fffcea1e2689d754f6425d/rotkehlchen/tests/fixtures/thegraph.py#L13'>rotkehlchen/tests/fixtures/thegraph.py:13:5:</a> PT004 Fixture `fixture_add_subgraph_api_key` does not return anything, add leading underscore
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT004 | 3 | 0 | 3 | 0 | 0 |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-11-16 19:49_

Ecosystem report analysis:
- The `freedomofpress` and `rotki` codebases see a few errors going away as a result of these removals.
- All other hits are TOML parse errors due to the fact that various repos had the rules explicitly ignored before now (which feels like it validates the choice to remove these rules)

---

_Comment by @AlexWaygood on 2024-11-16 19:53_

Same concern as https://github.com/astral-sh/ruff/pull/14384#issuecomment-2480758746 about merging this now...

---

_@MichaReiser approved on 2024-11-16 21:36_

---

_Label `do-not-merge` added by @AlexWaygood on 2024-11-16 21:44_

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-11-18 11:40_

---

_@MichaReiser reviewed on 2024-11-19 08:42_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:233 on 2024-11-19 08:42_

```suggestion
/// This rule has been removed because marking fixtures that do not return a value with an underscore
```

---

_Label `do-not-merge` removed by @MichaReiser on 2024-11-19 08:46_

---

_Merged by @MichaReiser on 2024-11-19 09:01_

---

_Closed by @MichaReiser on 2024-11-19 09:01_

---

_Branch deleted on 2024-11-19 09:01_

---
