```yaml
number: 19759
title: ":bug: Fix: rule (SIM114) was incorrectly removing comments when mergi…"
type: pull_request
state: closed
author: mikeleppane
labels:
  - bug
  - fixes
assignees: []
base: main
head: fix(#19576)/safe-fix-removes-comments
created_at: 2025-08-05T11:03:54Z
updated_at: 2025-11-21T08:06:47Z
url: https://github.com/astral-sh/ruff/pull/19759
synced_at: 2026-01-12T15:56:46Z
```

# :bug: Fix: rule (SIM114) was incorrectly removing comments when mergi…

---

_@mikeleppane_

…ng consecutive if/elif branches with identical bodies.

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

### Problem
The `if_with_same_arms` rule [(SIM114)](https://docs.astral.sh/ruff/rules/if-with-same-arms/) was incorrectly removing comments when merging consecutive if/elif branches with identical bodies. This could lead to loss of important documentation and developer intent.

Example of the issue (see the related issue for another example):
```python
if x == 1:
    return True
    # important comment 1
elif x == 2:
    return True
    # important comment 2
```
Previously merged to:
```python
if x == 1 or x == 2:
    return True  # important comment 2
```

### Solution
Added comprehensive comment preservation logic that:

* Preserves inline comments - Comments on the same line as test conditions are maintained
* Handles identical body comments - When both branches have identical comments, they're preserved in the merged result
* Prevents unsafe merges - Disables automatic fixing when comments would be lost, while still reporting the diagnostic

### Changes Made
* Added `can_preserve_comments_during_merge()` - Analyzes comment safety before merging
* Enhanced `merge_branches()` - Now only runs when comments can be safely preserved
* Improved `if_with_same_arms()` - Reports diagnostics even when automatic fixes are disabled


### Allowed Cases 

1. No comments
```python
 if x == 1:
    return True
elif x == 2:
    return True
```
Result: Merges to if x == 1 or x == 2:
2. Inline Comments with Test Conditions
```python
if x == 1:  # check for one
    return True
elif x == 2:  # check for two
    return True
```
Result: Merges to if x == 1 or x == 2:  # check for one
3. Identical Body Comments
```python
if x == 1:
    return True  # always return true
    # end comment
elif x == 2:
    return True  # always return true
    # end comment
```
Result: Merges and preserves the identical body comments.
4. Empty/Trivial Comments
```python
if x == 1:
    return True
    #
elif x == 2:
    return True
    #
```
Result: Merges successfully (empty comments are ignored).
5. Mixed Safe Comments
```python
if x == 1:  # inline comment
    return True  # body comment
elif x == 2:  # another inline
    return True  # body comment
```
=> Result:
```python
if x == 1 or x == 2:  # inline comment
    return True  # body comment
```

### Not Allowed Cases 
1. Any Standalone Comments Between Branches
```python
if x == 1:
    return True

# This is a standalone comment explaining something
elif x == 2:
    return True
```
Result: Diagnostic reported but no automatic fix.

2. Different Body Comments
```python
if x == 1:
    return True  # case one behavior
elif x == 2:
    return True  # case two behavior
```
Result: No automatic fix. Why: Body comments differ, so information would be lost.

3. Multiple Standalone Comments
```python
if x == 1:
    return True

# First explanation
# Second explanation
elif x == 2:
    return True
```
Result: No automatic fix. Why: Multiple standalone comments are too complex to safely preserve.


Related issue: [#19576](https://github.com/astral-sh/ruff/issues/19576)

## Test Plan

<!-- How was it tested? -->

Added a few extra snapshot test cases.


NOTE: `clippy` fails locally regarding `ty_server`:
```bash
error: unused import: `insta::assert_compact_debug_snapshot`
 --> crates\ty_server\tests\e2e\pull_diagnostics.rs:2:5
  |
2 | use insta::assert_compact_debug_snapshot;
  |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
  = note: `-D unused-imports` implied by `-D warnings`
  = help: to override `-D warnings` add `#[allow(unused_imports)]`

error: could not compile `ty_server` (test "e2e") due to 1 previous error
warning: build failed, waiting for other jobs to finish...
```


---

_Review requested from @carljm by @mikeleppane on 2025-08-05 11:03_

---

_Review requested from @MichaReiser by @mikeleppane on 2025-08-05 11:03_

---

_Review requested from @sharkdp by @mikeleppane on 2025-08-05 11:03_

---

_Review requested from @dcreager by @mikeleppane on 2025-08-05 11:03_

---

_Comment by @github-actions[bot] on 2025-08-05 11:05_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-05 11:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-08-05 11:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+18 -0 violations, +0 -6 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/c152b8709c38d5af6309cefa0fef573bea7ce0c0/src/plasmapy/formulary/collisions/lengths.py#L289'>src/plasmapy/formulary/collisions/lengths.py:289:5:</a> SIM114 Combine `if` branches using logical `or` operator
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1362dfb0069d504db4180a4b651062cb0f3915e7/airflow-core/src/airflow/ti_deps/deps/not_previously_skipped_dep.py#L70'>airflow-core/src/airflow/ti_deps/deps/not_previously_skipped_dep.py:70:17:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/apache/airflow/blob/1362dfb0069d504db4180a4b651062cb0f3915e7/airflow-core/src/airflow/utils/log/file_task_handler.py#L644'>airflow-core/src/airflow/utils/log/file_task_handler.py:644:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/apache/airflow/blob/1362dfb0069d504db4180a4b651062cb0f3915e7/airflow-core/tests/unit/models/test_backfill.py#L455'>airflow-core/tests/unit/models/test_backfill.py:455:5:</a> SIM114 Combine `if` branches using logical `or` operator
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_renderer.py#L282'>src/bokeh/plotting/_renderer.py:282:17:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_renderer.py#L284'>src/bokeh/plotting/_renderer.py:284:17:</a> SIM114 [*] Combine `if` branches using logical `or` operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_renderer.py#L286'>src/bokeh/plotting/_renderer.py:286:17:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/serialization.py#L190'>src/bokeh/util/serialization.py:190:5:</a> SIM114 Combine `if` branches using logical `or` operator
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/serialization.py#L190'>src/bokeh/util/serialization.py:190:5:</a> SIM114 [*] Combine `if` branches using logical `or` operator
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/fc82ad9f28eb3fe1fd4f00d03f35650e9874b7f4/mlflow/crewai/autolog.py#L181'>mlflow/crewai/autolog.py:181:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/mlflow/mlflow/blob/fc82ad9f28eb3fe1fd4f00d03f35650e9874b7f4/mlflow/pydantic_ai/autolog.py#L155'>mlflow/pydantic_ai/autolog.py:155:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/mlflow/mlflow/blob/fc82ad9f28eb3fe1fd4f00d03f35650e9874b7f4/mlflow/transformers/__init__.py#L2706'>mlflow/transformers/__init__.py:2706:9:</a> SIM114 Combine `if` branches using logical `or` operator
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+8 -0 violations, +0 -4 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/lib/attachments.py#L194'>zerver/lib/attachments.py:194:13:</a> SIM114 Combine `if` branches using logical `or` operator
- <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/lib/attachments.py#L194'>zerver/lib/attachments.py:194:13:</a> SIM114 [*] Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/lib/events.py#L1763'>zerver/lib/events.py:1763:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/lib/events.py#L1766'>zerver/lib/events.py:1766:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/lib/events.py#L1769'>zerver/lib/events.py:1769:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/lib/events.py#L1772'>zerver/lib/events.py:1772:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/lib/events.py#L1820'>zerver/lib/events.py:1820:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/lib/push_notifications.py#L784'>zerver/lib/push_notifications.py:784:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/lib/thumbnail.py#L597'>zerver/lib/thumbnail.py:597:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/views/documentation.py#L105'>zerver/views/documentation.py:105:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/views/registration.py#L755'>zerver/views/registration.py:755:17:</a> SIM114 Combine `if` branches using logical `or` operator
- <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/views/registration.py#L755'>zerver/views/registration.py:755:17:</a> SIM114 [*] Combine `if` branches using logical `or` operator
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM114 | 24 | 18 | 0 | 0 | 6 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+18 -0 violations, +0 -6 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/c152b8709c38d5af6309cefa0fef573bea7ce0c0/src/plasmapy/formulary/collisions/lengths.py#L289'>src/plasmapy/formulary/collisions/lengths.py:289:5:</a> SIM114 Combine `if` branches using logical `or` operator
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1362dfb0069d504db4180a4b651062cb0f3915e7/airflow-core/src/airflow/ti_deps/deps/not_previously_skipped_dep.py#L70'>airflow-core/src/airflow/ti_deps/deps/not_previously_skipped_dep.py:70:17:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/apache/airflow/blob/1362dfb0069d504db4180a4b651062cb0f3915e7/airflow-core/src/airflow/utils/log/file_task_handler.py#L644'>airflow-core/src/airflow/utils/log/file_task_handler.py:644:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/apache/airflow/blob/1362dfb0069d504db4180a4b651062cb0f3915e7/airflow-core/tests/unit/models/test_backfill.py#L455'>airflow-core/tests/unit/models/test_backfill.py:455:5:</a> SIM114 Combine `if` branches using logical `or` operator
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_renderer.py#L282'>src/bokeh/plotting/_renderer.py:282:17:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_renderer.py#L284'>src/bokeh/plotting/_renderer.py:284:17:</a> SIM114 [*] Combine `if` branches using logical `or` operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_renderer.py#L286'>src/bokeh/plotting/_renderer.py:286:17:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/serialization.py#L190'>src/bokeh/util/serialization.py:190:5:</a> SIM114 Combine `if` branches using logical `or` operator
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/serialization.py#L190'>src/bokeh/util/serialization.py:190:5:</a> SIM114 [*] Combine `if` branches using logical `or` operator
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/fc82ad9f28eb3fe1fd4f00d03f35650e9874b7f4/mlflow/crewai/autolog.py#L181'>mlflow/crewai/autolog.py:181:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/mlflow/mlflow/blob/fc82ad9f28eb3fe1fd4f00d03f35650e9874b7f4/mlflow/pydantic_ai/autolog.py#L155'>mlflow/pydantic_ai/autolog.py:155:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/mlflow/mlflow/blob/fc82ad9f28eb3fe1fd4f00d03f35650e9874b7f4/mlflow/transformers/__init__.py#L2706'>mlflow/transformers/__init__.py:2706:9:</a> SIM114 Combine `if` branches using logical `or` operator
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+8 -0 violations, +0 -4 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/lib/attachments.py#L194'>zerver/lib/attachments.py:194:13:</a> SIM114 Combine `if` branches using logical `or` operator
- <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/lib/attachments.py#L194'>zerver/lib/attachments.py:194:13:</a> SIM114 [*] Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/lib/events.py#L1763'>zerver/lib/events.py:1763:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/lib/events.py#L1766'>zerver/lib/events.py:1766:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/lib/events.py#L1769'>zerver/lib/events.py:1769:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/lib/events.py#L1772'>zerver/lib/events.py:1772:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/lib/events.py#L1820'>zerver/lib/events.py:1820:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/lib/push_notifications.py#L784'>zerver/lib/push_notifications.py:784:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/lib/thumbnail.py#L597'>zerver/lib/thumbnail.py:597:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/views/documentation.py#L105'>zerver/views/documentation.py:105:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/views/registration.py#L755'>zerver/views/registration.py:755:17:</a> SIM114 Combine `if` branches using logical `or` operator
- <a href='https://github.com/zulip/zulip/blob/68b205775ec5a0a51949c906cc0f9c89bf3df99d/zerver/views/registration.py#L755'>zerver/views/registration.py:755:17:</a> SIM114 [*] Combine `if` branches using logical `or` operator
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM114 | 24 | 18 | 0 | 0 | 6 |

</p>
</details>




---

_Review requested from @ntBre by @ntBre on 2025-08-08 14:07_

---

_Review request for @dcreager removed by @ntBre on 2025-08-08 14:07_

---

_Review request for @carljm removed by @ntBre on 2025-08-08 14:07_

---

_Review request for @MichaReiser removed by @ntBre on 2025-08-08 14:07_

---

_Review request for @sharkdp removed by @ntBre on 2025-08-08 14:07_

---

_Label `bug` added by @ntBre on 2025-08-12 19:02_

---

_Label `fixes` added by @ntBre on 2025-08-12 19:02_

---

_@ntBre reviewed on 2025-08-12 19:22_

Thank you for your work on this!

How different are the results here from using a comment check like this that is common in other rules:

https://github.com/astral-sh/ruff/blob/6f42c0d143460adf833caff139c11e9eaff7752d/crates/ruff_linter/src/rules/flake8_async/rules/async_zero_sleep.rs#L138-L146

That's the kind of solution I was picturing here when I saw the issue: either offer an unsafe fix if any comments would be deleted (just like the example above), or we can even return without a fix in those cases if the fix is "too" unsafe. I would kind of prefer that over a more complicated bespoke solution, if we can get most of the same benefits that way.

We're also raising new diagnostics in the ecosystem check that I think should be suppressed. They appear to fall under the previous check for identical bodies with different comments that should be preserved.

---

_Comment by @MichaReiser on 2025-11-21 08:06_

Thanks for your contribution. I'll close this PR due to inactivity, but we'd be more than happy to review a resubmission that answers Brent's question.

---

_Closed by @MichaReiser on 2025-11-21 08:06_

---
