```yaml
number: 18963
title: "[`pandas`] Avoid flagging `PD002` if `pandas` is not imported"
type: pull_request
state: merged
author: jordyjwilliams
labels:
  - rule
assignees: []
merged: true
base: main
head: 6432_pandas_on_non_pandas_fix
created_at: 2025-06-26T15:13:09Z
updated_at: 2025-06-27T10:31:38Z
url: https://github.com/astral-sh/ruff/pull/18963
synced_at: 2026-01-10T18:39:09Z
```

# [`pandas`] Avoid flagging `PD002` if `pandas` is not imported

---

_Pull request opened by @jordyjwilliams on 2025-06-26 15:13_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
* As per [the docs on PD002](https://docs.astral.sh/ruff/rules/pandas-use-of-inplace-argument/#pandas-use-of-inplace-argument-pd002)
    * Should only apply to `pandas` dataframes. 
    * As [noted here](https://github.com/astral-sh/ruff/issues/6432#issuecomment-2986380797) by @MeGaGiGaGon `PD002`  is still triggering.
    * Missed [in this PR](https://github.com/astral-sh/ruff/pull/14671)
    * This leverages `seen_module` that was introduced there.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
* Added new test cases to reproduce the issue of a `non pandas` import throwing.

### Current `main`
* Fails added tests.

### With new changes
* Tests pass.


# Appendix 

<details>
<summary>Added negative case tests failing on `main`</summary>


```rs
failures:

---- rules::pandas_vet::tests::contents::r_import_polars_as_pl_x_pl_dataframe_x_drop_a_inplace_true_pd002_pass_polars_expects stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot file: crates/ruff_linter/src/rules/pandas_vet/snapshots/ruff_linter__rules__pandas_vet__tests__PD002_pass_polars.snap
Snapshot: PD002_pass_polars
Source: crates/ruff_linter/src/rules/pandas_vet/mod.rs:373
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
          0 │+<filename>:4:15: PD002 [*] `inplace=True` should be avoided; it has inconsistent behavior
          1 │+  |
          2 │+2 | import polars as pl
          3 │+3 | x = pl.DataFrame()
          4 │+4 | x.drop(['a'], inplace=True)
          5 │+  |               ^^^^^^^^^^^^ PD002
          6 │+  |
          7 │+  = help: Assign to variable; remove `inplace` arg
          8 │+
          9 │+ℹ Unsafe fix
         10 │+1 1 |
         11 │+2 2 | import polars as pl
         12 │+3 3 | x = pl.DataFrame()
         13 │+4   |-x.drop(['a'], inplace=True)
         14 │+  4 |+x = x.drop(['a'])
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.

thread 'rules::pandas_vet::tests::contents::r_import_polars_as_pl_x_pl_dataframe_x_drop_a_inplace_true_pd002_pass_polars_expects' panicked at /Users/jordywilliams/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/insta-1.43.1/src/runtime.rs:679:13:
snapshot assertion for 'PD002_pass_polars' failed in line 373
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- rules::pandas_vet::tests::contents::r_x_dataframe_x_drop_a_inplace_true_pd002_pass_no_import_expects stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot file: crates/ruff_linter/src/rules/pandas_vet/snapshots/ruff_linter__rules__pandas_vet__tests__PD002_pass_no_import.snap
Snapshot: PD002_pass_no_import
Source: crates/ruff_linter/src/rules/pandas_vet/mod.rs:373
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
          0 │+<filename>:3:15: PD002 [*] `inplace=True` should be avoided; it has inconsistent behavior
          1 │+  |
          2 │+2 | x = DataFrame()
          3 │+3 | x.drop(['a'], inplace=True)
          4 │+  |               ^^^^^^^^^^^^ PD002
          5 │+  |
          6 │+  = help: Assign to variable; remove `inplace` arg
          7 │+
          8 │+ℹ Unsafe fix
          9 │+1 1 |
         10 │+2 2 | x = DataFrame()
         11 │+3   |-x.drop(['a'], inplace=True)
         12 │+  3 |+x = x.drop(['a'])
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.

thread 'rules::pandas_vet::tests::contents::r_x_dataframe_x_drop_a_inplace_true_pd002_pass_no_import_expects' panicked at /Users/jordywilliams/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/insta-1.43.1/src/runtime.rs:679:13:
snapshot assertion for 'PD002_pass_no_import' failed in line 373


failures:
    rules::pandas_vet::tests::contents::r_import_polars_as_pl_x_pl_dataframe_x_drop_a_inplace_true_pd002_pass_polars_expects
    rules::pandas_vet::tests::contents::r_x_dataframe_x_drop_a_inplace_true_pd002_pass_no_import_expects

test result: FAILED. 2442 passed; 2 failed; 4 ignored; 0 measured; 0 filtered out; finished in 0.93s

error: test failed, to rerun pass `-p ruff_linter --lib`
```

</details>

<!-- How was it tested? -->



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pandas_vet/rules/inplace_argument.rs`:56 on 2025-06-26 19:04_

The comment is now outdated.

Can you tell me more why we remove this check entirely?

CC: @dhruvmanila 

---

_@MichaReiser reviewed on 2025-06-26 19:05_

---

_Comment by @github-actions[bot] on 2025-06-26 19:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/ede3de0ca0addde195199be332d6e4dd454955ef/tests/integration_tests/model_tests.py#L491'>tests/integration_tests/model_tests.py:491:29:</a> PD002 `inplace=True` should be avoided; it has inconsistent behavior
+ <a href='https://github.com/apache/superset/blob/ede3de0ca0addde195199be332d6e4dd454955ef/tests/unit_tests/pandas_postprocessing/test_rename.py#L65'>tests/unit_tests/pandas_postprocessing/test_rename.py:65:9:</a> PD002 `inplace=True` should be avoided; it has inconsistent behavior
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PD002 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/ede3de0ca0addde195199be332d6e4dd454955ef/tests/integration_tests/model_tests.py#L491'>tests/integration_tests/model_tests.py:491:29:</a> PD002 `inplace=True` should be avoided; it has inconsistent behavior
+ <a href='https://github.com/apache/superset/blob/ede3de0ca0addde195199be332d6e4dd454955ef/tests/unit_tests/pandas_postprocessing/test_rename.py#L65'>tests/unit_tests/pandas_postprocessing/test_rename.py:65:9:</a> PD002 `inplace=True` should be avoided; it has inconsistent behavior
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PD002 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_@ntBre approved on 2025-06-26 19:19_

Looks good, thanks!

Edit: I didn't see Micha's review, I'll defer to him and Dhruv.

---

_@jordyjwilliams reviewed on 2025-06-26 22:50_

---

_Review comment by @jordyjwilliams on `crates/ruff_linter/src/rules/pandas_vet/rules/inplace_argument.rs`:56 on 2025-06-26 22:50_

> The comment is now outdated.
> 
> Can you tell me more why we remove this check entirely?
> 
> CC: @dhruvmanila 

Fair call, I will remove the comment (https://github.com/astral-sh/ruff/pull/18963/commits/7348b1caefa5b2a0903c4d2f8703a782ae29e458). I removed the check for the following reasons:
* To align better with the code styling and implementation from: https://github.com/astral-sh/ruff/pull/14671
* As noted in that PR yes this may give false negatives. But this is preferred to false positives (less end-user gripe)
* I believe the check is now unnecessary given the additional tests added
    * On current `main` these would fail, eg they would trigger the `pandas linter` issue.
    * With these changes, they now pass.
    * All existing tests pass, thus no existing functionality (that I'm aware of) will change.


If there's something I'm missing here please let me know. Happy to add back in the check.

---

_Comment by @jordyjwilliams on 2025-06-26 22:51_

> Looks good, thanks!
> 
> Edit: I didn't see Micha's review, I'll defer to him and Dhruv.

Noted. Will wait until Micha approves or has concerns addressed.

---

_@jordyjwilliams reviewed on 2025-06-26 23:20_

---

_Review comment by @jordyjwilliams on `crates/ruff_linter/src/rules/pandas_vet/rules/inplace_argument.rs`:56 on 2025-06-26 23:20_

EG with these changes we are using the `cheker` however in a different way (as introduced in #14671.

`!checker.semantic().seen_module(Modules::PANDAS)` we will now `skip the check` `pandas` has not been seen (used) within the offending code. 

Thus; this will allow for less false positives with similar codes on `in-place` operations. eg `pl.dataframe`

---

_@dhruvmanila reviewed on 2025-06-27 06:00_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pandas_vet/rules/inplace_argument.rs`:56 on 2025-06-27 06:00_

@MichaReiser Isn't this change in line with what you did in https://github.com/astral-sh/ruff/pull/14671? In other words, why wasn't that change applied to this rule?

---

_@dhruvmanila reviewed on 2025-06-27 06:03_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pandas_vet/rules/inplace_argument.rs`:56 on 2025-06-27 06:03_

The existing check would only work when the method is used directly like `pandas.DataFrame.sort_values` and not when used as `df = pandas.DataFrame(); df.sort_values`.

---

_@MichaReiser reviewed on 2025-06-27 06:53_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pandas_vet/rules/inplace_argument.rs`:56 on 2025-06-27 06:53_

> The existing check would only work when the method is used directly like pandas.DataFrame.sort_values and not when used as df = pandas.DataFrame(); df.sort_values.

Yeah, I think that's the main difference. This PR removes some false positives (when panda isn't enabled at all) but introduces some new false positives (when using panda but the method doesn't come from pandas). This is different from my PRs where I only added the check.

However, I think this is still fine because it is in line with all other pandas rules.

---

_Label `rule` added by @MichaReiser on 2025-06-27 06:53_

---

_Merged by @MichaReiser on 2025-06-27 06:56_

---

_Closed by @MichaReiser on 2025-06-27 06:56_

---

_@jordyjwilliams reviewed on 2025-06-27 06:58_

---

_Review comment by @jordyjwilliams on `crates/ruff_linter/src/rules/pandas_vet/rules/inplace_argument.rs`:56 on 2025-06-27 06:58_

@MichaReiser good to merge from your point of view here? What I guess I was going for here was standardization. 

And also basically looking to commit to this repo for the first time. @dhruvmanila's suggestion is what I was implementing here for robustness, completeness and to close out https://github.com/astral-sh/ruff/issues/6432#issuecomment-2986380797

In my experience people who would use `pandas.DataFrame` would typically have these as variables and run `my_df.sort_values(in_place=True)` or so... Thus I figured it's better to pick up on these cases (when we know `pandas` has been seen)... And rather have all `pd` methods work the same.

---

_Branch deleted on 2025-06-27 06:58_

---

_@MichaReiser reviewed on 2025-06-27 06:59_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pandas_vet/rules/inplace_argument.rs`:56 on 2025-06-27 06:59_

Yeah, I think it's fine. Thank you for working on this! 

---

_Review comment by @jordyjwilliams on `crates/ruff_linter/src/rules/pandas_vet/rules/inplace_argument.rs`:56 on 2025-06-27 07:01_

Nws, thanks for the approval and merge. 

Anything further required from my side after merge? 

I have read [the contributing guide](https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md) just trying to make sure there's nothing I've missed here?

---

_@jordyjwilliams reviewed on 2025-06-27 07:01_

---

_@MichaReiser reviewed on 2025-06-27 07:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pandas_vet/rules/inplace_argument.rs`:56 on 2025-06-27 07:02_

No, all good from your side. This will go out in the next release

---

_Renamed from "[pandas]: Fix issue on `non pandas` dataframe `in-place` usage (PD002)" to "[`pandas`] Avoid flagging `PD002` if `pandas` is not imported" by @dhruvmanila on 2025-06-27 10:31_

---
