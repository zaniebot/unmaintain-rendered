```yaml
number: 14887
title: "[`airflow`] Add fix to remove deprecated keyword arguments (`AIR302`)"
type: pull_request
state: merged
author: tirkarthi
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: air302-fixes
created_at: 2024-12-10T06:13:35Z
updated_at: 2024-12-10T14:21:35Z
url: https://github.com/astral-sh/ruff/pull/14887
synced_at: 2026-01-12T15:55:49Z
```

# [`airflow`] Add fix to remove deprecated keyword arguments (`AIR302`)

---

_@tirkarthi_

## Summary

Add replacement fixes to deprecated arguments of a DAG.

Ref #14582 #14626

## Test Plan

Diff was verified and snapshots were updated.


---

_Comment by @tirkarthi on 2024-12-10 06:14_

cc: @uranusjr @Lee-W 

---

_Comment by @github-actions[bot] on 2024-12-10 06:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -3 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/391ad6bbf20d86d2aa18b0605738706df22bf579/dev/perf/dags/perf_dag_1.py#L41'>dev/perf/dags/perf_dag_1.py:41:10:</a> AIR302 `airflow.operators.bash_operator.BashOperator` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/391ad6bbf20d86d2aa18b0605738706df22bf579/dev/perf/dags/perf_dag_1.py#L41'>dev/perf/dags/perf_dag_1.py:41:10:</a> AIR302 `airflow.operators.bash_operator.BashOperator` is removed in Airflow 3.0; use `airflow.operators.bash.BashOperator` instead
+ <a href='https://github.com/apache/airflow/blob/391ad6bbf20d86d2aa18b0605738706df22bf579/dev/perf/dags/perf_dag_1.py#L48'>dev/perf/dags/perf_dag_1.py:48:12:</a> AIR302 `airflow.operators.bash_operator.BashOperator` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/391ad6bbf20d86d2aa18b0605738706df22bf579/dev/perf/dags/perf_dag_1.py#L48'>dev/perf/dags/perf_dag_1.py:48:12:</a> AIR302 `airflow.operators.bash_operator.BashOperator` is removed in Airflow 3.0; use `airflow.operators.bash.BashOperator` instead
+ <a href='https://github.com/apache/airflow/blob/391ad6bbf20d86d2aa18b0605738706df22bf579/performance/src/performance_dags/performance_dag/performance_dag.py#L244'>performance/src/performance_dags/performance_dag/performance_dag.py:244:9:</a> AIR302 [*] `schedule_interval` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/391ad6bbf20d86d2aa18b0605738706df22bf579/performance/src/performance_dags/performance_dag/performance_dag.py#L244'>performance/src/performance_dags/performance_dag/performance_dag.py:244:9:</a> AIR302 `schedule_interval` is removed in Airflow 3.0; use `schedule` instead
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR302 | 6 | 3 | 3 | 0 | 0 |

</p>
</details>




---

_Label `fixes` added by @MichaReiser on 2024-12-10 07:16_

---

_Label `preview` added by @MichaReiser on 2024-12-10 07:16_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302_args.py.snap`:33 on 2024-12-10 07:19_

What do you think of using "use `schedule` instead` as help text and remove it from the message

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:67 on 2024-12-10 07:19_

We should return `None` if there's no replacement (fix). It's otherwise confusing when we suggest that they replace the deprecated keyword but there's no replacement because it was removed.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:95 on 2024-12-10 07:20_

We should expand the rule documentation and explain why the fix is unsafe. 

---

_@MichaReiser reviewed on 2024-12-10 07:21_

Thank you

---

_@dhruvmanila reviewed on 2024-12-10 08:56_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:76 on 2024-12-10 08:56_

Let's not wrap the `Diagnostic` in a `Some` here. That way you don't need to unwrap it with `if let` below. So,
```rust
let keyword = arguments.find_keyword(deprecated)?;
let mut diagnostic = Diagnostic::new(...);
diagnostic.set_fix(...);
Some(diagnostic)
```

---

_@dhruvmanila reviewed on 2024-12-10 08:58_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:92 on 2024-12-10 08:58_

As mentioned earlier, we should use [`remove_argument`](https://github.com/astral-sh/ruff/blob/e3f34b8f5bd1eb715e732e5a277aacedac17358b/crates/ruff_linter/src/fix/edits.rs#L207) when `replacement` is `None` which is what that indicates.

---

_Renamed from "[Airflow] Add automatic fixes to AIR302 argument rules" to "[`airflow`] Add unsafe fix for deprecated keyword arguments (`AIR302`)" by @dhruvmanila on 2024-12-10 08:59_

---

_@MichaReiser reviewed on 2024-12-10 09:01_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:92 on 2024-12-10 09:01_

I'm a bit worried about offering a fix to remove an argument because a proper fix might require more changes to preserve the Airflow 2 behavior.

---

_@dhruvmanila reviewed on 2024-12-10 09:09_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:92 on 2024-12-10 09:09_

Hm, I think you're right. We can skip that for now.

---

_Review comment by @tirkarthi on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:92 on 2024-12-10 09:19_

Thanks, as per my understanding `sla_miss_callback` has `None::<&str>` as `replacement` but since the parameter is removed in Airlfow 3 as per code I guess it should move to `removed_name` and `diagnostic_for_argument` should receive a `replacement: &str` . Happy to hear thoughts from @uranusjr .

https://github.com/apache/airflow/blob/490b5e816b804f338b0eb97f240ae874d4e15810/task_sdk/src/airflow/sdk/definitions/dag.py#L317

---

_@tirkarthi reviewed on 2024-12-10 09:19_

---

_@tirkarthi reviewed on 2024-12-10 09:21_

---

_Review comment by @tirkarthi on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302_args.py.snap`:33 on 2024-12-10 09:21_

Agreed, it makes it cleaner to show what is the error in the message and `help` acting as a how to fix it.

```
cargo run -p ruff -- check --preview --select=AIR302 example_latest_only.py --no-cache        

example_latest_only.py:30:5: AIR302 `schedule_interval` is removed in Airflow 3.0.
   |
28 | with DAG(
29 |     dag_id="latest_only",
30 |     schedule_interval="daily",
   |     ^^^^^^^^^^^^^^^^^ AIR302
31 |     start_date=datetime.datetime(2021, 1, 1),
32 |     catchup=False,
   |
   = help: Use `schedule` instead.

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

---

_@tirkarthi reviewed on 2024-12-10 09:41_

---

_Review comment by @tirkarthi on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:67 on 2024-12-10 09:41_

Thanks, done.

---

_@tirkarthi reviewed on 2024-12-10 09:42_

---

_Review comment by @tirkarthi on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:95 on 2024-12-10 09:42_

The fixes are drop in replacements for keywords. I had them unsafe by default for review. Is there a convention over how to classify if a fix as safe or unsafe?

---

_@tirkarthi reviewed on 2024-12-10 09:42_

---

_Review comment by @tirkarthi on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:76 on 2024-12-10 09:42_

Thanks, done.

---

_@Lee-W reviewed on 2024-12-10 09:56_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:92 on 2024-12-10 09:56_

If we want to fix something, we probably can make it another function?

---

_@dhruvmanila reviewed on 2024-12-10 10:01_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:92 on 2024-12-10 10:01_

I think the `removed_name` function is mainly for imported symbols like functions, classes and variables so we can't really use that.

I would suggest we skip it for now and can visit later. The main reason is that removing just the argument isn't really that useful given that the references will still be present in the function body and we can't determine reliably for how to remove those references. As a user, I'd find it more useful to use the argument itself to find all the references, remove them and then remove the argument itself.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:57 on 2024-12-10 10:04_

The convention is to not use a period in user facing messages, you'll need to update the snapshots

```suggestion
            Replacement::Name(_) => {
                format!("`{deprecated}` is removed in Airflow 3.0")
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:68 on 2024-12-10 10:05_

same as above
```suggestion
            Some(format!("Use `{name}` instead"))
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:97 on 2024-12-10 10:05_

```suggestion
    if let Some(replacement) = replacement {
        diagnostic.set_fix(Fix::unsafe_edit(Edit::range_replacement(
            replacement.to_string(),
```

---

_@dhruvmanila approved on 2024-12-10 10:06_

Thank you for working on this! There are a few small changes but otherwise this looks good to go. Welcome to Ruff!

---

_@dhruvmanila reviewed on 2024-12-10 10:11_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:95 on 2024-12-10 10:11_

What Micha is referring to is to add a section like https://docs.astral.sh/ruff/rules/unused-import/#fix-safety that describes why the fix is marked as unsafe. You can use existing documentation as a reference, search for "Fix safety" at https://docs.astral.sh/ruff/rules.

I think the reason this is unsafe is because the user would still need to update the references to the argument in the function body.

---

_Review comment by @tirkarthi on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:95 on 2024-12-10 10:59_

Thanks for the link @dhruvmanila , I feel the fixes are safer in this case since the values to keyword arguments or the attributes are not really referenced anywhere. I have marked the fixes as `safe`.

---

_@tirkarthi reviewed on 2024-12-10 10:59_

---

_Renamed from "[`airflow`] Add unsafe fix for deprecated keyword arguments (`AIR302`)" to "[`airflow`] Add fix for deprecated keyword arguments (`AIR302`)" by @tirkarthi on 2024-12-10 11:00_

---

_@tirkarthi reviewed on 2024-12-10 11:00_

---

_Review comment by @tirkarthi on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:57 on 2024-12-10 11:00_

Thanks, updated and will follow the convention in future PRs.

---

_@dhruvmanila reviewed on 2024-12-10 11:19_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:95 on 2024-12-10 11:19_

> keyword arguments or the attributes are not really referenced anywhere

I'm not sure I understand this, can you expand? The argument most likely is being used in the function body, I'm referring to those references.

---

_@tirkarthi reviewed on 2024-12-10 11:50_

---

_Review comment by @tirkarthi on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:95 on 2024-12-10 11:50_

The keyword arguments are to construction of a dag. Then the task constructed inside the context manager are automatically attached to the dag. They are not referred by the tasks in the context manager body and are specific to the dag object itself.

Below is a dag called `latest_only` where there are two tasks `latest_only` and `task1` where `task1` depends on `latest_only` and is denoted by overloading `>>` operator. Airflow earlier used to have `schedule_interval` and `timetable` but then they were merged to `schedule` keyword argument which is compatible to accept values and is a drop in replacement.

Before

```python
with DAG(
    dag_id="latest_only",
    schedule_interval="daily",
    start_date=datetime.datetime(2021, 1, 1),
    catchup=False,
    tags=["example2", "example3"],
    sla_miss_callback=sla_callback
) as dag:
    latest_only = LatestOnlyOperator(task_id="latest_only")
    task1 = EmptyOperator(task_id="task1")

    latest_only >> task1

```

After fixes

```python
with DAG(
    dag_id="latest_only",
    schedule="daily",
    start_date=datetime.datetime(2021, 1, 1),
    catchup=False,
    tags=["example2", "example3"],
    sla_miss_callback=sla_callback
) as dag:
    latest_only = LatestOnlyOperator(task_id="latest_only")
    task1 = EmptyOperator(task_id="task1")

    latest_only >> task1

```

---

_@dhruvmanila reviewed on 2024-12-10 13:17_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:95 on 2024-12-10 13:17_

Oh right, sorry I misunderstood. These are function call arguments and not function parameters. Yeah, I think these should be safe to fix.

---

_Renamed from "[`airflow`] Add fix for deprecated keyword arguments (`AIR302`)" to "[`airflow`] Add fix to remove deprecated keyword arguments (`AIR302`)" by @dhruvmanila on 2024-12-10 13:19_

---

_Merged by @dhruvmanila on 2024-12-10 13:19_

---

_Closed by @dhruvmanila on 2024-12-10 13:19_

---

_Comment by @tirkarthi on 2024-12-10 14:21_

Thanks @dhruvmanila and @MichaReiser for the review and merge.

---
