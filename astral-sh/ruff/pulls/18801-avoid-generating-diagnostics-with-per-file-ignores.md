```yaml
number: 18801
title: Avoid generating diagnostics with per-file ignores
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - diagnostics
assignees: []
merged: true
base: main
head: brent/earlier-per-file-ignores
created_at: 2025-06-19T19:30:37Z
updated_at: 2025-06-20T17:33:11Z
url: https://github.com/astral-sh/ruff/pull/18801
synced_at: 2026-01-12T15:56:25Z
```

# Avoid generating diagnostics with per-file ignores

---

_@ntBre_

## Summary

This PR avoids one of the three calls to `NoqaCode::rule` from https://github.com/astral-sh/ruff/pull/18391 by applying per-file ignores in the `LintContext`. To help with this, it also replaces all direct uses of `LinterSettings.rules.enabled` with a `LintContext::enabled` (or `Checker::enabled`, which defers to its context) method. There are still some direct accesses to `settings.rules`, but as far as I can tell these are not in a part of the code where we can really access a `LintContext`. I believe all of the code reachable from `check_path`, where the replaced per-file ignore code was, should be converted to the new methods.

## Test Plan

Existing tests, with a single snapshot updated for RUF100, which I think actually shows a more accurate diagnostic message now.


---

_Label `internal` added by @ntBre on 2025-06-19 19:30_

---

_Label `diagnostics` added by @ntBre on 2025-06-19 19:30_

---

_Comment by @github-actions[bot] on 2025-06-19 19:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+8 -1 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/tests/unit/models/test_renderedtifields.py#L205'>airflow-core/tests/unit/models/test_renderedtifields.py:205:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/tests/unit/serialization/serializers/test_serializers.py#L249'>airflow-core/tests/unit/serialization/serializers/test_serializers.py:249:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/providers/google/tests/unit/google/cloud/hooks/test_bigquery.py#L157'>providers/google/tests/unit/google/cloud/hooks/test_bigquery.py:157:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/providers/google/tests/unit/google/cloud/hooks/test_bigquery.py#L158'>providers/google/tests/unit/google/cloud/hooks/test_bigquery.py:158:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/providers/google/tests/unit/google/cloud/hooks/test_bigquery_system.py#L40'>providers/google/tests/unit/google/cloud/hooks/test_bigquery_system.py:40:13:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/providers/google/tests/unit/google/cloud/hooks/test_bigquery_system.py#L44'>providers/google/tests/unit/google/cloud/hooks/test_bigquery_system.py:44:13:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/providers/weaviate/tests/unit/weaviate/hooks/test_weaviate.py#L393'>providers/weaviate/tests/unit/weaviate/hooks/test_weaviate.py:393:9:</a> PLC0415 `import` should be at the top-level of a file
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/fc1b6a73820b98768568a65555d5a88748e32f1b/ibis/backends/tests/signature/typecheck.py#L8'>ibis/backends/tests/signature/typecheck.py:8:1:</a> RUF100 Unused `noqa` directive (non-enabled: `D205`, `D415`, `D400`)
- <a href='https://github.com/ibis-project/ibis/blob/fc1b6a73820b98768568a65555d5a88748e32f1b/ibis/backends/tests/signature/typecheck.py#L8'>ibis/backends/tests/signature/typecheck.py:8:1:</a> RUF100 Unused `noqa` directive (unused: `D205`, `D415`, `D400`)
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC0415 | 7 | 7 | 0 | 0 | 0 |
| RUF100 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+8 -1 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/tests/unit/models/test_renderedtifields.py#L205'>airflow-core/tests/unit/models/test_renderedtifields.py:205:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/tests/unit/serialization/serializers/test_serializers.py#L249'>airflow-core/tests/unit/serialization/serializers/test_serializers.py:249:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/providers/google/tests/unit/google/cloud/hooks/test_bigquery.py#L157'>providers/google/tests/unit/google/cloud/hooks/test_bigquery.py:157:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/providers/google/tests/unit/google/cloud/hooks/test_bigquery.py#L158'>providers/google/tests/unit/google/cloud/hooks/test_bigquery.py:158:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/providers/google/tests/unit/google/cloud/hooks/test_bigquery_system.py#L40'>providers/google/tests/unit/google/cloud/hooks/test_bigquery_system.py:40:13:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/providers/google/tests/unit/google/cloud/hooks/test_bigquery_system.py#L44'>providers/google/tests/unit/google/cloud/hooks/test_bigquery_system.py:44:13:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/providers/weaviate/tests/unit/weaviate/hooks/test_weaviate.py#L393'>providers/weaviate/tests/unit/weaviate/hooks/test_weaviate.py:393:9:</a> PLC0415 `import` should be at the top-level of a file
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/fc1b6a73820b98768568a65555d5a88748e32f1b/ibis/backends/tests/signature/typecheck.py#L8'>ibis/backends/tests/signature/typecheck.py:8:1:</a> RUF100 Unused `noqa` directive (non-enabled: `D205`, `D415`, `D400`)
- <a href='https://github.com/ibis-project/ibis/blob/fc1b6a73820b98768568a65555d5a88748e32f1b/ibis/backends/tests/signature/typecheck.py#L8'>ibis/backends/tests/signature/typecheck.py:8:1:</a> RUF100 Unused `noqa` directive (unused: `D205`, `D415`, `D400`)
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC0415 | 7 | 7 | 0 | 0 | 0 |
| RUF100 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @codspeed-hq[bot] on 2025-06-19 19:39_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fearlier-per-file-ignores?runnerMode=Instrumentation)

### Merging #18801 will **not alter performance**

<sub>Comparing <code>brent/earlier-per-file-ignores</code> (0e7769f) with <code>main</code> (073a71c)</sub>



### Summary

`✅ 37` untouched benchmarks  





---

_Comment by @ntBre on 2025-06-19 19:55_

I'm a bit confused by both the ecosystem and benchmark results. It's at least plausible that I broke per-file ignores, but I don't see PLC0415 in Airflow's main `pyproject.toml`, at least. And I'm really surprised that these changes could be slower since they should be skipping the creation of diagnostics that previously would have been created and then discarded. We clone the `RuleTable` if there are any per-file ignores, but I didn't expect that to be too expensive.

---

_@MichaReiser reviewed on 2025-06-19 20:01_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:478 on 2025-06-19 20:01_

Oh, removing the const here could be problematic because the `RuleTable` lookup requires some bit operations and I assume that they might no longer happen at comp time

---

_@MichaReiser reviewed on 2025-06-19 20:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:3121 on 2025-06-19 20:02_

I wonder if it's cheaper to just clone the rule table. It's just a large array or we could consider using an `Rc` instead

---

_@ntBre reviewed on 2025-06-19 20:03_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:478 on 2025-06-19 20:03_

Ahhh, hopefully that's it. I'll try just always cloning, the `Cow` is the reason I removed these consts.

---

_@MichaReiser reviewed on 2025-06-19 20:04_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/noqa.rs`:232 on 2025-06-19 20:04_

Hmm. The behavior here might be different? It explicitly checked if the rule is disabled in the per file ignores (not sure why?)

---

_@ntBre reviewed on 2025-06-19 20:06_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/noqa.rs`:232 on 2025-06-19 20:06_

I cloned the airflow repo and I'm not getting any hits for PLC0415 with this branch locally. Hopefully it will go away when I push another commit. I thought `enabled` should have that folded in now, but maybe there is a subtle difference.

---

_@ntBre reviewed on 2025-06-19 20:35_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:478 on 2025-06-19 20:35_

I guess that was it, thank you! Now there's a very slight speedup.

---

_@MichaReiser reviewed on 2025-06-19 20:38_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:478 on 2025-06-19 20:38_

Yeah, the rule checks are in a very hot path. Still surprising that it would regress by that much

---

_Comment by @MichaReiser on 2025-06-19 20:43_

We override the rule selection for airflow to all, see https://github.com/astral-sh/ruff/blob/10a78fbc3b04e896e494d177fa408bce45e09ff4/python/ruff-ecosystem/ruff_ecosystem/defaults.py#L23-L26 Not sure why we see this difference.

You can also see the command in the ecosystem results:

```
ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL
```

---

_@MichaReiser reviewed on 2025-06-19 20:47_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:252 on 2025-06-19 20:47_

Nit: This reads a bit strange. It gives the impression that we iterate over diagnostics (the enabled diagnostics). 

I'm inclined to rename `diagnostics` to `context`, and rename all methods to `is_rule_enabled`, `iter_enabled_rules`...

---

_Comment by @ntBre on 2025-06-19 20:48_

Thanks, I can reproduce this locally now. There are 2579 hits for PLC0415 on 0.12, so I'll try to see if there's anything different about the 7 new ones.

---

_Comment by @ntBre on 2025-06-19 21:08_

I think the Airflow hits have to do with PLC0415 checking if TID253 is enabled:

https://github.com/astral-sh/ruff/blob/beff9c766aeabc4e827405bfdbc46b1a120dd9b7/crates/ruff_linter/src/rules/pylint/rules/import_outside_top_level.rs#L65-L69

Airflow _does_ have per-file [ignores](https://github.com/apache/airflow/blob/275a81a30a11d4484cdf227cd582fe467d195ca0/pyproject.toml#L746-L750) for TID253 that cover all of the new hits. It seems like `--select ALL` was taking precedence before, but now `per-file-ignores` is taking precedence, disabling TID253 and causing these diagnostics. Does that sound right to you, or was the old behavior correct?

That also explains why I wasn't reproducing this with just `--select PLC0415`, you have to select both rules, or `ALL` obviously.

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:252 on 2025-06-19 21:09_

I agree, I'm not sure why I called this `diagnostics` in the first place...

---

_@ntBre reviewed on 2025-06-19 21:09_

---

_Comment by @MichaReiser on 2025-06-19 21:11_

> I think the Airflow hits have to do with PLC0415 checking if TID253 is enabled:

This does sounds plossible and, judging by the comment, the new behavior seems more correct

---

_@ntBre reviewed on 2025-06-19 21:13_

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:252 on 2025-06-19 21:13_

Should I rename `Checker::enabled` and `Checker::any_enabled` too? I was trying to be consistent with those for the method names.

---

_@MichaReiser reviewed on 2025-06-19 21:16_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:252 on 2025-06-19 21:16_

I think so. It's not `checker` that's enabled, it's whether a rule is enabled.

---

_Marked ready for review by @ntBre on 2025-06-19 21:59_

---

_Review requested from @AlexWaygood by @ntBre on 2025-06-19 21:59_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-06-19 21:59_

---

_Label `internal` removed by @MichaReiser on 2025-06-20 07:34_

---

_Label `bug` added by @MichaReiser on 2025-06-20 07:34_

---

_@MichaReiser reviewed on 2025-06-20 07:43_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:3122 on 2025-06-20 07:43_

I feel like this check should be inside `ignores_from_path`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:3117 on 2025-06-20 07:43_

I found it somewhat convenient that `LintContext` stores the settings, as I think it will allow us to pass `LintContext` in most places where we currently pass `Checker`. 

But I guess it makes sense not to store the settings if we also store them on `Checker`, although I think we should just remove it from `Checker`. Either way. This seems unrelated to this PR and not urgent but maybe something for a contributor issue.

---

_@MichaReiser approved on 2025-06-20 07:45_

Nice!

---

_@ntBre reviewed on 2025-06-20 12:23_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:3117 on 2025-06-20 12:23_

That makes sense. I just thought it might be confusing to have access to a `RulesTable` through either `self.settings.rules` or just `self.rules`. These fields are all private anyway, though. 

If we want to keep the settings on the context, it might be convenient to partially revert the removal here, otherwise someone will have to plumb all the lifetimes back through. The rest sounds like a good follow-up like you said.

---

_@ntBre reviewed on 2025-06-20 16:38_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:3117 on 2025-06-20 16:38_

Okay I restored the settings field with an `expect(unused)` and a TODO, just to preserve all of the lifetimes for a follow-up. I can open an issue too and possibly tag it help-wanted.

---

_Merged by @ntBre on 2025-06-20 17:33_

---

_Closed by @ntBre on 2025-06-20 17:33_

---

_Branch deleted on 2025-06-20 17:33_

---
