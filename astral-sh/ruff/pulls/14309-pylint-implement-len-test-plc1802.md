```yaml
number: 14309
title: "`[pylint]` Implement `len-test` (`PLC1802`)"
type: pull_request
state: merged
author: Lokejoke
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: use-implicit-booleaness
created_at: 2024-11-13T08:37:57Z
updated_at: 2024-11-26T19:30:18Z
url: https://github.com/astral-sh/ruff/pull/14309
synced_at: 2026-01-10T20:50:57Z
```

# `[pylint]` Implement `len-test` (`PLC1802`)

---

_Pull request opened by @Lokejoke on 2024-11-13 08:37_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR implements [`use-implicit-booleaness-not-len` / `C1802`](https://pylint.pycqa.org/en/latest/user_guide/messages/convention/use-implicit-booleaness-not-len.html)
> For sequences, (strings, lists, tuples), use the fact that empty sequences are false. 


<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan


Checked against pylint tests:
 * I would argue some of the tests (commented out) are not very intuitive and thus not helpful 
<!-- How was it tested? -->

## Notes
* **Naming**: Rule name and its error message (adopted from pylint) do not follow conventions used in Ruff (`len-as-condition` might be solution) 

## References
[PEP 8 on empty sequences and implicit booleaness](https://peps.python.org/pep-0008/#programming-recommendations)

---

_Converted to draft by @Lokejoke on 2024-11-13 08:47_

---

_Comment by @github-actions[bot] on 2024-11-13 10:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+21 -0 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/d8c91aa6195104a5cf994fc2086d31d6743a6e40/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L1639'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:1639:8:</a> PLC1802 [*] `len(success_entries)` used as condition without comparison
+ <a href='https://github.com/apache/airflow/blob/d8c91aa6195104a5cf994fc2086d31d6743a6e40/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L1646'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:1646:8:</a> PLC1802 [*] `len(skipped_entries)` used as condition without comparison
+ <a href='https://github.com/apache/airflow/blob/d8c91aa6195104a5cf994fc2086d31d6743a6e40/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L1751'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:1751:12:</a> PLC1802 [*] `len(success_entries)` used as condition without comparison
+ <a href='https://github.com/apache/airflow/blob/d8c91aa6195104a5cf994fc2086d31d6743a6e40/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L1758'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:1758:12:</a> PLC1802 [*] `len(skipped_entries)` used as condition without comparison
+ <a href='https://github.com/apache/airflow/blob/d8c91aa6195104a5cf994fc2086d31d6743a6e40/providers/src/airflow/providers/amazon/aws/hooks/glue.py#L264'>providers/src/airflow/providers/amazon/aws/hooks/glue.py:264:16:</a> PLC1802 [*] `len(fetched_logs)` used as condition without comparison
+ <a href='https://github.com/apache/airflow/blob/d8c91aa6195104a5cf994fc2086d31d6743a6e40/providers/src/airflow/providers/sftp/sensors/sftp.py#L129'>providers/src/airflow/providers/sftp/sensors/sftp.py:129:16:</a> PLC1802 [*] `len(files_found)` used as condition without comparison
+ <a href='https://github.com/apache/airflow/blob/d8c91aa6195104a5cf994fc2086d31d6743a6e40/providers/src/airflow/providers/snowflake/operators/snowflake.py#L560'>providers/src/airflow/providers/snowflake/operators/snowflake.py:560:20:</a> PLC1802 [*] `len(queries_in_progress)` used as condition without comparison
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/97683ec05297fc6eeffde08b02063aaa8223eff3/superset/databases/filters.py#L98'>superset/databases/filters.py:98:16:</a> PLC1802 [*] `len(allowed_schemas)` used as condition without comparison
+ <a href='https://github.com/apache/superset/blob/97683ec05297fc6eeffde08b02063aaa8223eff3/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L444'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:444:61:</a> PLC1802 [*] `len(SEQUENCE)` used as condition without comparison
+ <a href='https://github.com/apache/superset/blob/97683ec05297fc6eeffde08b02063aaa8223eff3/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L546'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:546:11:</a> PLC1802 [*] `len(ordered_raw_positions)` used as condition without comparison
+ <a href='https://github.com/apache/superset/blob/97683ec05297fc6eeffde08b02063aaa8223eff3/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L559'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:559:16:</a> PLC1802 [*] `len(available_columns_index)` used as condition without comparison
+ <a href='https://github.com/apache/superset/blob/97683ec05297fc6eeffde08b02063aaa8223eff3/superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py#L68'>superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py:68:24:</a> PLC1802 [*] `len(keys)` used as condition without comparison
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/validation/check.py#L216'>src/bokeh/core/validation/check.py:216:27:</a> PLC1802 [*] `len(warnings)` used as condition without comparison
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/c4c1edfa6e7a9dfd1f7bb85e5481d89492bb1e8c/zerver/views/invite.py#L127'>zerver/views/invite.py:127:8:</a> PLC1802 [*] `len(streams)` used as condition without comparison
+ <a href='https://github.com/zulip/zulip/blob/c4c1edfa6e7a9dfd1f7bb85e5481d89492bb1e8c/zerver/views/invite.py#L243'>zerver/views/invite.py:243:8:</a> PLC1802 [*] `len(streams)` used as condition without comparison
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/acf130311806cb1a627ea0e21369c7d6f586663a/testing/_py/test_local.py#L218'>testing/_py/test_local.py:218:16:</a> PLC1802 [*] `len(lst)` used as condition without comparison
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/b282736f251abe29e2777110ba03a4939c6af955/astropy/io/ascii/misc.py#L83'>astropy/io/ascii/misc.py:83:12:</a> PLC1802 [*] `len(first)` used as condition without comparison
+ <a href='https://github.com/astropy/astropy/blob/b282736f251abe29e2777110ba03a4939c6af955/astropy/units/core.py#L1341'>astropy/units/core.py:1341:16:</a> PLC1802 [*] `len(subresults)` used as condition without comparison
+ <a href='https://github.com/astropy/astropy/blob/b282736f251abe29e2777110ba03a4939c6af955/astropy/units/core.py#L2501'>astropy/units/core.py:2501:12:</a> PLC1802 [*] `len(names)` used as condition without comparison
+ <a href='https://github.com/astropy/astropy/blob/b282736f251abe29e2777110ba03a4939c6af955/astropy/wcs/wcs.py#L2950'>astropy/wcs/wcs.py:2950:16:</a> PLC1802 [*] `len(missing_keys)` used as condition without comparison
+ <a href='https://github.com/astropy/astropy/blob/b282736f251abe29e2777110ba03a4939c6af955/astropy/wcs/wcs.py#L3765'>astropy/wcs/wcs.py:3765:20:</a> PLC1802 [*] `len(content)` used as condition without comparison
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC1802 | 21 | 21 | 0 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @Lokejoke on 2024-11-13 10:55_

---

_Renamed from "[pylint] use-implicit-booleaness-not-len (`C1802`)" to "[pylint] Implement use-implicit-booleaness-not-len (`C1802`)" by @Lokejoke on 2024-11-14 09:59_

---

_Renamed from "[pylint] Implement use-implicit-booleaness-not-len (`C1802`)" to "`[pylint]` Implement use-implicit-booleaness-not-len (`C1802`)" by @Lokejoke on 2024-11-14 09:59_

---

_Renamed from "`[pylint]` Implement use-implicit-booleaness-not-len (`C1802`)" to "`[pylint]` Implement `use-implicit-booleaness-not-len` (`C1802`)" by @Lokejoke on 2024-11-14 10:00_

---

_Renamed from "`[pylint]` Implement `use-implicit-booleaness-not-len` (`C1802`)" to "`[pylint]` Implement `use-implicit-booleaness-not-len` (`PLC1802`)" by @Lokejoke on 2024-11-14 10:00_

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/pylint/len_as_condition.py`:1 on 2024-11-14 16:15_

Can you say more about why you commented out some of pylint's tests? Do they not pass in the Ruff implementation, and/or do you think the lint is incorrect in these cases?

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/pylint/len_as_condition.py`:1 on 2024-11-14 16:20_

Could you test some more complicated examples involving rebindings/scope since you use those properties of the semantic model? Maybe something like:

```python
def f(cond:bool):
    x = [1,2,3]
    if cond:
        x = [4,5,6]
    if len(x):
        print(x)
```

and

```python
def g(cond:bool):
    x = [1,2,3]
    if cond:
        x = [4,5,6]
    if len(x):
        print(x)
    del x
```

and

```python
def h(cond:bool):
    x = [1,2,3]
    x = 123
    if len(x):
        print(x)
```
and

```python
def outer():
    x = [1,2,3]
    def inner(x:int):
        return x+1
    if len(x):
        print(x)
```

or any others you can think of to exercise the implementation.


---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/len_as_condition.rs`:46 on 2024-11-14 16:23_

can you reword as "used as condition without comparison"?

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/len_as_condition.rs`:34 on 2024-11-14 16:25_

Can you add a reference to PEP 8? specifically this section: https://peps.python.org/pep-0008/#programming-recommendations

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/len_as_condition.rs`:18 on 2024-11-14 16:30_

The parenthetical examples are a little confusing: the first is equivalent to `if len(x) == 0` and the second is equivalent to `if x`. I think you should pick one type of example here or else omit the parenthetical comments.

Also - why the double backticks?

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/len_as_condition.rs`:20 on 2024-11-14 16:30_

Can you add the `not` versions of this example too?

---

_@dylwil3 requested changes on 2024-11-14 16:33_

Thanks so much for this contribution! Some minor nits/requests for a few more tests. I also didn't get a chance to review the implementation carefully, but I'll hold off for now in case the new tests inspire any changes.

---

_Label `rule` added by @dylwil3 on 2024-11-14 17:09_

---

_Label `preview` added by @dylwil3 on 2024-11-14 17:09_

---

_@Lokejoke reviewed on 2024-11-15 11:54_

---

_Review comment by @Lokejoke on `crates/ruff_linter/resources/test/fixtures/pylint/len_as_condition.py`:1 on 2024-11-15 11:54_

I've commented out the code i have mixed feelings about:
* object implicit boolean (first using '__bool__', then '__len__'), removing call might change the output
* than the function tests (maybe after stronger type inference)

I'll change it, so tests would show how they differ from pylint   

---

_@Lokejoke reviewed on 2024-11-15 12:56_

---

_Review comment by @Lokejoke on `crates/ruff_linter/resources/test/fixtures/pylint/len_as_condition.py`:1 on 2024-11-15 12:56_

Good point, this implementation does support only one name assignment in current scope.
It would be fair to check all prior assignments to this. 

---

_Review requested from @dylwil3 by @Lokejoke on 2024-11-15 16:50_

---

_Assigned to @dylwil3 by @dylwil3 on 2024-11-17 19:56_

---

_@Lokejoke reviewed on 2024-11-19 11:48_

---

_Review comment by @Lokejoke on `crates/ruff_linter/resources/test/fixtures/pylint/len_as_condition.py`:1 on 2024-11-19 11:48_

I'm unsure, there are 3 options:
* check only those without rebindings (as is now)
* check recursively if all of possible rebindings bounds to a sequence-like object
* leave it for type inference

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/len_as_condition.rs`:152 on 2024-11-21 02:52_

Do generators have a length? I think `len((x for x in range(10))` gives a `TypeError`, whereas `(x for x in range(10))` is truthy, so these would not have the same behavior in a boolean test.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/len_as_condition.rs`:152 on 2024-11-21 02:53_

Do generators have a length? I think `len((x for x in range(10))` gives a `TypeError`, whereas `(x for x in range(10))` is truthy, so these would not have the same behavior in a boolean test.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/len_as_condition.rs`:128 on 2024-11-21 03:05_

Nice! Should we also see if its bound to `kwargs`? Those should have a length too.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/len_as_condition.rs`:167 on 2024-11-21 03:08_

How about `range`, `str`, `repr`, or other builtins that return sets, dicts, etc.?

---

_@dylwil3 requested changes on 2024-11-21 03:26_

This looks really great, thank you! I think even with the possible misses around control flow, this makes sense to implement. You would have to be pretty pathological to create a custom class `C` that implements `__len__` and `__bool__` where `bool(x) != bool(len(x))` for `x: C`... so I think it's okay.

---

_@Lokejoke reviewed on 2024-11-22 09:34_

---

_Review comment by @Lokejoke on `crates/ruff_linter/src/rules/pylint/rules/len_as_condition.rs`:167 on 2024-11-22 09:34_

I'm gonna explore that, good point.


---

_Renamed from "`[pylint]` Implement `use-implicit-booleaness-not-len` (`PLC1802`)" to "`[pylint]` Implement `len-as-condition` (`PLC1802`)" by @Lokejoke on 2024-11-25 17:48_

---

_@Lokejoke reviewed on 2024-11-25 18:20_

---

_Review comment by @Lokejoke on `crates/ruff_linter/src/rules/pylint/rules/len_as_condition.rs`:152 on 2024-11-25 18:20_

Yes, it throws an error. Generators should be excluded from the search.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/len_as_condition.rs`:184 on 2024-11-25 22:49_

I think a few of these return generators (so again the user would've had `len` incorrectly, causing an error), namely:

- sorted
- enumerate
- zip

also maybe we're missing:

- bin
- bytes
- bytearray
- hex
- memoryview
- oct

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/len_as_condition.rs`:152 on 2024-11-25 22:52_

Just realized we're missing `PythonType::Bytes` here - sorry I didn't catch it before!

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/len_as_condition.rs`:144 on 2024-11-25 22:53_

Can you update this comment?

---

_@dylwil3 requested changes on 2024-11-25 23:17_

Almost there! Looking good!

---

_Comment by @MichaReiser on 2024-11-26 08:34_

Naming: Maybe `len-test` to align with https://docs.astral.sh/ruff/rules/not-in-test/#not-in-test-e713

---

_Renamed from "`[pylint]` Implement `len-as-condition` (`PLC1802`)" to "`[pylint]` Implement `len-test (`PLC1802`)" by @Lokejoke on 2024-11-26 10:13_

---

_Review requested from @dylwil3 by @Lokejoke on 2024-11-26 10:14_

---

_Renamed from "`[pylint]` Implement `len-test (`PLC1802`)" to "`[pylint]` Implement `len-test` (`PLC1802`)" by @Lokejoke on 2024-11-26 10:26_

---

_@dylwil3 approved on 2024-11-26 19:28_

LGTM, thank you for the great contribution and your patience through all the reviews!

---

_Merged by @dylwil3 on 2024-11-26 19:30_

---

_Closed by @dylwil3 on 2024-11-26 19:30_

---
