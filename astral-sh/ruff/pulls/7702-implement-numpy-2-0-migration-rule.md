```yaml
number: 7702
title: Implement NumPy 2.0 migration rule
type: pull_request
state: merged
author: mtsokol
labels:
  - rule
assignees: []
merged: true
base: main
head: npy201
created_at: 2023-09-28T20:58:14Z
updated_at: 2023-11-03T19:09:06Z
url: https://github.com/astral-sh/ruff/pull/7702
synced_at: 2026-01-10T23:40:55Z
```

# Implement NumPy 2.0 migration rule

---

_Pull request opened by @mtsokol on 2023-09-28 20:58_

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Hi! Currently NumPy Python API is undergoing a cleanup process that will be delivered in NumPy 2.0 (release is planned for the end of the year). 
Most changes are rather simple (renaming, removing or moving a member of the main namespace to a new place), and they could be flagged/fixed by an additional ruff rule for numpy (e.g. changing occurrences of `np.float_` to `np.float64`).

Would you accept such rule?  

I named it `NPY201` in the existing group, so people will receive a heads-up for changes arriving in 2.0 before actually migrating to it.

~~This is still a draft PR.~~ I'm not an expert in rust so if any part of code can be done better please share!

NumPy 2.0 migration guide: https://numpy.org/devdocs/numpy_2_0_migration_guide.html
NEP 52: https://numpy.org/neps/nep-0052-python-api-cleanup.html
NumPy cleanup tracking issue: https://github.com/numpy/numpy/issues/23999


## Test Plan

<!-- How was it tested? -->

A unit test is provided that checks all rule's fix cases. 


---

_Comment by @charliermarsh on 2023-09-28 22:43_

Happy to include something like this. I need to think a bit more on whether this should be its own rule, or whether we should just extend `deprecated_function` (and add a `deprecated_constant` for the remaining rules).

As of what NumPy version are these fixes safe? As in, are there older, supported versions of NumPy for which applying these rules would cause issues? (Ultimately asking: will Ruff need to know the NumPy version?)

---

_Comment by @mtsokol on 2023-09-28 23:21_

Thank you for the feedback! [All these breaking changes](https://numpy.org/devdocs/numpy_2_0_migration_guide.html#main-namespace) will be shipped in 2.0.0 version that is planned for this December. The latest release of numpy is 1.26.0. 

Only nightly 2.0.0.dev0 and future 2.0.0 versions are safe to apply these changes. 
For past releases these are incompatible ones, therefore we thought to establish it as a separate rule. Downstream users that are still on 1.x would upgrade ruff and get a heads-up errors, which they can suppress by disabling the rule. 
Once they decide to upgrade to 2.0 they can run ruff with `--fix` and get most of changes done (and these renames can be mundane to do manually - I did them for numpy, scipy, etc. and it was magnitude of hundreds of lines of changed code).

If ruff would know numpy version then it could be used to determine whether to apply this rule. 

---

_Comment by @charliermarsh on 2023-10-01 03:18_

Thanks @mtsokol. Sorry if I'm misunderstanding, but _some_ of these fixes would be safe to apply in older versions, right? For example, the rules that suggest using `np.inf` would be safe to apply, since `np.inf` exists in older versions, etc.

---

_Comment by @mtsokol on 2023-10-01 07:55_

@charliermarsh Ah yes! Actually **most of changes** from the NumPy 2.0 [migration guide](https://numpy.org/devdocs/numpy_2_0_migration_guide.html) can be applied to the previous versions. (My last response was confusing - only a few are backward incompatible, e.g. renaming `np.int_` to `np.long` (https://github.com/numpy/numpy/pull/24794) or moving `np.byte_bounds` to `np.lib.array_utils`. Right now I'm working on these renames mostly and I forgot to mention that it's just a fraction of all changes).

`np.Infinity` -> `np.inf`, `np.float_` -> `np.float64` etc. are all backward compatible and are safe to apply to previous versions.

---

_Comment by @mtsokol on 2023-10-02 19:03_

The PR's blueprint is ready from my side - it consist of three types of AST fixes:
- `AutoImport`: the deprecated member of the numpy namespace can be replaced with another, e.g. `np.float_` with `np.float64` or `np.INF` with `np.inf`.
- `AutoPurePython`: the deprecated member can be replaced with python expression, e.g. `np.NZERO` with `-0.0` or `np.issubclass_` with `issubclass`.
- `Manual`: can't be automatically fixed - a guideline is provided on how to fix it.

If the code's shape is correct I will provision the rest of entries and exhaustive unit test. Please share your feedback!

P.S. I get some errors in the CI that I can't reproduce locally: 
```
  |
1 | use ruff_diagnostics::{AutofixKind, Diagnostic, Edit, Fix, Violation};
  |                        ^^^^^^^^^^^ no `AutofixKind` in the root
```
I followed implementation of other numpy rules. What am I missing?

---

_Marked ready for review by @mtsokol on 2023-10-02 19:03_

---

_Comment by @charliermarsh on 2023-10-02 19:40_

@mtsokol - We recently renamed `AutofixKind` to `AutofixAvailability`, so you might be running into something like: GitHub is rebasing your PR on main prior to testing, and so the name doesn't exist anymore.

---

_Comment by @mtsokol on 2023-10-02 19:56_

> @mtsokol - We recently renamed `AutofixKind` to `AutofixAvailability`, so you might be running into something like: GitHub is rebasing your PR on main prior to testing, and so the name doesn't exist anymore.

Thank you! Now CI tests are passing. 

---

_Comment by @codspeed-hq[bot] on 2023-10-02 19:57_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtsokol:npy201)

### Merging #7702 will **not alter performance**

<sub>Comparing <code>mtsokol:npy201</code> (b86abcd) with <code>main</code> (f64c389)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_Comment by @github-actions[bot] on 2023-10-02 20:02_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+3, -1, 0 error(s))

<details><summary>airflow (+3, -1)</summary>
<p>

<pre>
- [*] 16167 fixable with the `--fix` option (6240 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 16169 fixable with the `--fix` option (6240 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ <a href='https://github.com/apache/airflow/blob/6316025b7c160006ef57d3463868394c3f9e8d97/airflow/serialization/serializers/numpy.py#L78'>airflow/serialization/serializers/numpy.py:78:13:</a> NPY201 [*] `np.float_` will be removed in the NumPy 2.0. Use `numpy.float64` instead.
+ <a href='https://github.com/apache/airflow/blob/6316025b7c160006ef57d3463868394c3f9e8d97/airflow/serialization/serializers/numpy.py#L78'>airflow/serialization/serializers/numpy.py:78:60:</a> NPY201 [*] `np.complex_` will be removed in the NumPy 2.0. Use `numpy.complex128` instead.
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| NPY201 | 2 | 2 | 0 |



---

_Comment by @hoxbro on 2023-10-06 20:51_

It looks like you are not changing NaN to nan. Per https://numpy.org/devdocs/numpy_2_0_migration_guide.html

---

_Comment by @mtsokol on 2023-10-06 21:31_

> It looks like you are not changing NaN to nan. Per https://numpy.org/devdocs/numpy_2_0_migration_guide.html

We're still finishing adding new changes but here it's just a matter of adding new entries to match in the rule (I will add the rest soon!). I was curious if the overall design of PR and three types of fixes proposed looks correct. 

---

_Comment by @mtsokol on 2023-10-24 10:36_

Hi @charliermarsh @Hoxbro,

We finished Python API refactoring for NumPy 2.0. I updated this PR - now the new rule contains all changes introduced to the main namespace (i.e. members' removals).

Out of all 52 cases in the rule only one is backward incompatible (`np.byte_bounds`). 51 remaining ones are relevant for both NumPy 1.x and 2.0 releases and contribute to cleaner codebase regardless of NumPy version used. There are 29 cases that can be auto-fixed and 23 that require manual intervention.

I added a unit test that checks all rule's fix cases. 


---

_Comment by @charliermarsh on 2023-10-24 14:09_

Thanks @mtsokol -- will queue this up for review and try to get to it shortly.

---

_Comment by @charliermarsh on 2023-10-27 15:51_

(Sorry for the delay on this -- we shipped our formatter earlier this week and I've been busy with follow-ups from the release. This is still on my list.)

---

_Comment by @charliermarsh on 2023-11-03 03:44_

This looks great, thanks @mtsokol! I like the way you structured the rule -- it reads well.

I took some liberties in editing the rule documentation and tweaking a few of the migration messages, with the goal of making them a little more consistent with the copy and voice we use in other rules. If you have any feedback or issues with the changes, just let me know, happy to address in a follow-up PR.

---

_Label `rule` added by @charliermarsh on 2023-11-03 03:44_

---

_Merged by @charliermarsh on 2023-11-03 03:47_

---

_Closed by @charliermarsh on 2023-11-03 03:47_

---

_Comment by @github-actions[bot] on 2023-11-03 03:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/db012ac5de1d65e40ca75b80c9e15e1c5ac4b224/airflow/providers/salesforce/hooks/salesforce.py#L263'>airflow/providers/salesforce/hooks/salesforce.py:263:34:</a> NPY201 [*] `np.NaN` will be removed in NumPy 2.0. Use `numpy.nan` instead.
+ <a href='https://github.com/apache/airflow/blob/db012ac5de1d65e40ca75b80c9e15e1c5ac4b224/airflow/serialization/serializers/numpy.py#L78'>airflow/serialization/serializers/numpy.py:78:13:</a> NPY201 [*] `np.float_` will be removed in NumPy 2.0. Use `numpy.float64` instead.
+ <a href='https://github.com/apache/airflow/blob/db012ac5de1d65e40ca75b80c9e15e1c5ac4b224/airflow/serialization/serializers/numpy.py#L78'>airflow/serialization/serializers/numpy.py:78:60:</a> NPY201 [*] `np.complex_` will be removed in NumPy 2.0. Use `numpy.complex128` instead.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| NPY201 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_Branch deleted on 2023-11-03 07:50_

---

_Comment by @hoxbro on 2023-11-03 07:53_

Shouldn't the new numpy imports happen at the same level as the existing numpy import? 

---

_Comment by @mtsokol on 2023-11-03 09:10_

@charliermarsh Hi, all tweaks look good to me - thank you for a review! 

---

_Comment by @pllim on 2023-11-03 13:50_

Hello! As a user who does not know much about ruff internals, how do I try this out downstream? Do I only have to modify https://github.com/astropy/astropy/blob/main/.ruff.toml ? If so, how? Is this documented somewhere?

---

_Comment by @charliermarsh on 2023-11-03 13:55_

@Hoxbro - Can you give an example of where this is happening?

---

_Comment by @charliermarsh on 2023-11-03 13:56_

@pllim -- You shouldn't need to modify your configuration, but you _would_ need to build from source, which just means that you need to have Rust installed. If you have Rust installed, then you should be able to run something like:
```shell
cargo run -p ruff_linter -- check ../path/to/astropy --select NPY --preview
```

---

_Review comment by @hoxbro on `crates/ruff_linter/src/rules/numpy/snapshots/ruff_linter__rules__numpy__tests__numpy2-deprecation_NPY201.py.snap`:18 on 2023-11-03 14:00_

@charliermarsh, here is an example. 

---

_@hoxbro reviewed on 2023-11-03 14:00_

---

_@charliermarsh reviewed on 2023-11-03 14:47_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/numpy/snapshots/ruff_linter__rules__numpy__tests__numpy2-deprecation_NPY201.py.snap`:18 on 2023-11-03 14:47_

I think that instead we should focus on trying to get this to use `np.lib.add_docstring` here.

---

_Comment by @pllim on 2023-11-03 15:05_

> but you would need to build from source

Oh, so this isn't one of those code I can just add to the TOML? Or maybe I can in the future when this feature is released?

Not sure if I want to install rust and build this from source on a CI for a Python package.

---

_@hoxbro reviewed on 2023-11-03 15:11_

---

_Review comment by @hoxbro on `crates/ruff_linter/src/rules/numpy/snapshots/ruff_linter__rules__numpy__tests__numpy2-deprecation_NPY201.py.snap`:18 on 2023-11-03 15:11_

I think this is a great way and will only change the line where changes are needed.  

Just needs to be sure that the different submodules are initialized when running `import numpy as np`. 

---

_Review comment by @hoxbro on `crates/ruff_linter/src/rules/numpy/snapshots/ruff_linter__rules__numpy__tests__numpy2-deprecation_NPY201.py.snap`:84 on 2023-11-03 15:15_

Sorry to be so noisy after the PR has been merged.  

I can't do this import in numpy 1.26.0. It is possible to do `from numpy.lib.utils import byte_bounds`

---

_@hoxbro reviewed on 2023-11-03 15:16_

---

_@mtsokol reviewed on 2023-11-03 15:18_

---

_Review comment by @mtsokol on `crates/ruff_linter/src/rules/numpy/snapshots/ruff_linter__rules__numpy__tests__numpy2-deprecation_NPY201.py.snap`:84 on 2023-11-03 15:18_

Hi! This is the only backward incompatible change in the rule, mentioned in the comment: https://github.com/astral-sh/ruff/pull/7702#issuecomment-1776953773. It will be available in `array_utils` from 2.0. 

---

_@hoxbro reviewed on 2023-11-03 15:29_

---

_Review comment by @hoxbro on `crates/ruff_linter/src/rules/numpy/snapshots/ruff_linter__rules__numpy__tests__numpy2-deprecation_NPY201.py.snap`:84 on 2023-11-03 15:29_

Then I'm really sorry about the noise :slightly_smiling_face:   

---

_@charliermarsh reviewed on 2023-11-03 15:38_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/numpy/snapshots/ruff_linter__rules__numpy__tests__numpy2-deprecation_NPY201.py.snap`:84 on 2023-11-03 15:38_

I should probably make this a suggested fix with a clear message.

---

_Comment by @charliermarsh on 2023-11-03 15:38_

@pllim -- It'll be available in the next release (it was just merged last night), so later today or early next week.

---

_Comment by @pllim on 2023-11-03 15:45_

> It'll be available in the next release

And after that, I can just edit my `.ruff.toml` to use this?

---

_Comment by @zanieb on 2023-11-03 15:46_

@pllim yep!

---

_Comment by @pllim on 2023-11-03 15:49_

What codes are available? Is there a page where I can look this up? Thanks!!

---

_Comment by @charliermarsh on 2023-11-03 15:54_

Everything on the [rules page](https://docs.astral.sh/ruff/rules/) is available in the latest release. (We only update the documentation when we release.)

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/numpy/snapshots/ruff_linter__rules__numpy__tests__numpy2-deprecation_NPY201.py.snap`:84 on 2023-11-03 16:07_

Fixed in https://github.com/astral-sh/ruff/pull/8474

---

_@charliermarsh reviewed on 2023-11-03 16:07_

---

_Comment by @pllim on 2023-11-03 19:09_

Great, thanks! Looking forward to the release.

---
