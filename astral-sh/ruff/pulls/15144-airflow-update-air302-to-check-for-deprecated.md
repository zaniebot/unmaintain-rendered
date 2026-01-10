```yaml
number: 15144
title: "[`airflow`] Update `AIR302` to check for deprecated context keys"
type: pull_request
state: merged
author: sunank200
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: deprecated_context_variable_airflow
created_at: 2024-12-26T08:14:27Z
updated_at: 2025-02-01T00:02:25Z
url: https://github.com/astral-sh/ruff/pull/15144
synced_at: 2026-01-10T19:57:22Z
```

# [`airflow`] Update `AIR302` to check for deprecated context keys

---

_Pull request opened by @sunank200 on 2024-12-26 08:14_

**Summary**

Airflow 3.0 removes a set of deprecated context variables that were phased out in 2.x. This PR introduces lint rules to detect usage of these removed variables in various patterns, helping identify incompatibilities. The removed context variables include:

```
conf
execution_date
next_ds
next_ds_nodash
next_execution_date
prev_ds
prev_ds_nodash
prev_execution_date
prev_execution_date_success
tomorrow_ds
yesterday_ds
yesterday_ds_nodash
```

**Detected Patterns and Examples**

The linter now flags the use of removed context variables in the following scenarios:

1. **Direct Subscript Access**  
   ```python
   execution_date = context["execution_date"]  # Flagged
   ```
   
2. **`.get("key")` Method Calls**  
   ```python
   print(context.get("execution_date"))  # Flagged
   ```
   
3. **Variables Assigned from `get_current_context()`**  
   If a variable is assigned from `get_current_context()` and then used to access a removed key:  
   ```python
   c = get_current_context()
   print(c.get("execution_date"))  # Flagged
   ```
   
4. **Function Parameters in `@task`-Decorated Functions**  
   Parameters named after removed context variables in functions decorated with `@task` are flagged:  
   ```python
   from airflow.decorators import task
   
   @task
   def my_task(execution_date, **kwargs):  # Parameter 'execution_date' flagged
       pass
   ```
   
5. **Removed Keys in Task Decorator `kwargs` and Other Scenarios**  
   Other similar patterns where removed context variables appear (e.g., as part of `kwargs` in a `@task` function) are also detected.
```
from airflow.decorators import task

@task
def process_with_execution_date(**context):
    execution_date = lambda: context["execution_date"]  # flagged
    print(execution_date)

@task(kwargs={"execution_date": "2021-01-01"})   # flagged
def task_with_kwargs(**context):  
    pass
```

**Test Plan**

Test fixtures covering various patterns of deprecated context usage are included in this PR. For example:

```python
from airflow.decorators import task, dag, get_current_context
from airflow.models import DAG
from airflow.operators.dummy import DummyOperator
import pendulum
from datetime import datetime

@task
def access_invalid_key_task(**context):
    print(context.get("conf"))  # 'conf' flagged

@task
def print_config(**context):
    execution_date = context["execution_date"]  # Flagged
    prev_ds = context["prev_ds"]                # Flagged

@task
def from_current_context():
    context = get_current_context()
    print(context["execution_date"])            # Flagged

# Usage outside of a task decorated function
c = get_current_context()
print(c.get("execution_date"))                 # Flagged

@task
def some_task(execution_date, **kwargs):
    print("execution date", execution_date)     # Parameter flagged

@dag(
    start_date=pendulum.datetime(2021, 1, 1, tz="UTC")
)
def my_dag():
    task1 = DummyOperator(
        task_id="task1",
        params={
            "execution_date": "{{ execution_date }}",  # Flagged in template context
        },
    )

    access_invalid_key_task()
    print_config()
    from_current_context()
    
dag = my_dag()

class CustomOperator(BaseOperator):
    def execute(self, context):
        execution_date = context.get("execution_date")                      # Flagged
        next_ds = context.get("next_ds")                                               # Flagged
        next_execution_date = context["next_execution_date"]          # Flagged
```

Ruff will emit `AIR302` diagnostics for each deprecated usage, with suggestions when applicable, aiding in code migration to Airflow 3.0.

related: https://github.com/apache/airflow/issues/44409, https://github.com/apache/airflow/issues/41641

---

_Comment by @github-actions[bot] on 2024-12-26 08:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -2 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/charts/schema_tests.py#L59'>tests/integration_tests/charts/schema_tests.py:59:9:</a> ANN201 Missing return type annotation for public function `test_query_context_series_limit`
- <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/charts/schema_tests.py#L59'>tests/integration_tests/charts/schema_tests.py:59:9:</a> ANN201 Missing return type annotation for public function `test_query_context_series_limit`
+ <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/viz_tests.py#L739'>tests/integration_tests/viz_tests.py:739:9:</a> ANN201 Missing return type annotation for public function `test_filter_nulls`
- <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/viz_tests.py#L739'>tests/integration_tests/viz_tests.py:739:9:</a> ANN201 Missing return type annotation for public function `test_filter_nulls`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ANN201 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -3 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/7a28f29842e4fa103466e8b49b2986389862f486/providers/tests/system/papermill/example_papermill_remote_verify.py#L44'>providers/tests/system/papermill/example_papermill_remote_verify.py:44:37:</a> AIR302 `execution_date` is removed in Airflow 3.0
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/charts/schema_tests.py#L59'>tests/integration_tests/charts/schema_tests.py:59:9:</a> PLR6301 Method `test_query_context_series_limit` could be a function, class method, or static method
+ <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/charts/schema_tests.py#L59'>tests/integration_tests/charts/schema_tests.py:59:9:</a> PLR6301 Method `test_query_context_series_limit` could be a function, class method, or static method
- <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/model_tests.py#L202'>tests/integration_tests/model_tests.py:202:9:</a> PLR6301 Method `test_adjust_engine_params_mysql` could be a function, class method, or static method
+ <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/model_tests.py#L202'>tests/integration_tests/model_tests.py:202:9:</a> PLR6301 Method `test_adjust_engine_params_mysql` could be a function, class method, or static method
+ <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/viz_tests.py#L899'>tests/integration_tests/viz_tests.py:899:9:</a> ANN201 Missing return type annotation for public function `test_apply_rolling_without_data`
- <a href='https://github.com/apache/superset/blob/6d117ffbb564f611df21fcb9a19c15b9567f35d9/tests/integration_tests/viz_tests.py#L899'>tests/integration_tests/viz_tests.py:899:9:</a> ANN201 Missing return type annotation for public function `test_apply_rolling_without_data`
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6301 | 4 | 2 | 2 | 0 | 0 |
| ANN201 | 2 | 1 | 1 | 0 | 0 |
| AIR302 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302_context.py.snap`:5 on 2024-12-26 08:48_

I'm not sure whether we really need to change the code to just add `Airflow context` here 🤔 we probably could just use message

---

_@uranusjr reviewed on 2024-12-26 08:48_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:58 on 2024-12-26 08:48_

Instead of an additional flag, it may make more sense to make this a separate case in the Replacement enum.

---

_@uranusjr reviewed on 2024-12-26 08:51_

---

_Review comment by @uranusjr on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:42 on 2024-12-26 08:51_

There are some more things we should check

1. Named keyword arguments (e.g. `def print_config(execution_date)`)
2. Getting the context with `get_current_context` instead of function arguments.
3. `context` in an operator’s `execute`.

These can be added in separate PRs instead after this is merged.

---

_Review comment by @Lee-W on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:44 on 2024-12-26 08:52_

@sunank200 and I discussed this earlier. What we're trying to check is whether there's a variable named as `context` in a function (most commonly seen in taskflow and python operator) and whether it's can be accessed like a dict with the keys we want to check. I think it's unlikely users are using something like this out of the airflow context. But would like to know whether there's any concern

@MichaReiser @uranusjr 

---

_Review comment by @kaxil on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:675 on 2024-12-26 08:52_

"conf" had also been removed

---

_@kaxil reviewed on 2024-12-26 08:52_

---

_Review comment by @Lee-W on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:45 on 2024-12-26 08:54_

@sunank200 I just think of there're other ways to access the context value like https://airflow.apache.org/docs/apache-airflow/stable/templates-ref.html#accessing-airflow-context-variables-from-taskflow-tasks. Do you think we should check it as well?

---

_@Lee-W reviewed on 2024-12-26 08:54_

---

_@Lee-W reviewed on 2024-12-26 08:57_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:675 on 2024-12-26 08:57_

It's tracked in https://github.com/apache/airflow/issues/45212, but yep, we could do it here as well

---

_@Lee-W reviewed on 2024-12-26 08:59_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:675 on 2024-12-26 08:59_

If we want to add `conf` here, it would be awesome if we could include `triggering_dataset_events` → `triggering_asset_events`. If not, I can make a separate PR

---

_@sunank200 reviewed on 2024-12-27 04:13_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:58 on 2024-12-27 04:13_

I have changed it.

---

_@sunank200 reviewed on 2024-12-27 04:44_

---

_Review comment by @sunank200 on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:45 on 2024-12-27 04:44_

I have added logic for other ways to access context value as well. It is part of tests. 

---

_@sunank200 reviewed on 2024-12-27 04:45_

---

_Review comment by @sunank200 on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:44 on 2024-12-27 04:45_

I have added logic for other ways to access context value as well. It is part of tests. 

---

_@sunank200 reviewed on 2024-12-27 04:45_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302_context.py.snap`:5 on 2024-12-27 04:45_

Changed it

---

_@sunank200 reviewed on 2024-12-27 04:45_

---

_Review comment by @sunank200 on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:42 on 2024-12-27 04:45_

I have added logic for other ways to access context value as well. It is part of tests. 

---

_@sunank200 reviewed on 2024-12-27 04:46_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:675 on 2024-12-27 04:46_

I have added `conf` removal check. i`nclude triggering_dataset_events` → `triggering_asset_events` can be part of another PR

---

_Review requested from @Lee-W by @sunank200 on 2024-12-27 04:53_

---

_Review requested from @uranusjr by @sunank200 on 2024-12-27 04:54_

---

_@uranusjr reviewed on 2024-12-27 05:35_

---

_Review comment by @uranusjr on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:44 on 2024-12-27 05:35_

It’s probably better to detect

1. Arguments of a function decorated with `@task` (either `**` or simple named arguments). (As a follow-up, any functions called by such a function)
2. The `execute` function of a BaseOperator subclass (As a follow-up, any functions called by `execute`)
3. The dict returned by `get_current_context`.

This should be better than detecting with variable name.

---

_@uranusjr reviewed on 2024-12-27 05:37_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:18 on 2024-12-27 05:37_

Simply `Context` or `ContextItem` is proabaly enough. I see the mesage provided here is always `is removed in Airflow 3.0.`. Do we plan to add other messages? If not, we can probably just hard-code it in Violation instead.

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:965 on 2024-12-27 05:38_

I think we can do better with `?` and some kind of `map` thingy but I’m not fluent in Rust enough to make a concrete suggestion…

---

_@uranusjr reviewed on 2024-12-27 05:38_

---

_Review comment by @Lee-W on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:44 on 2024-12-27 05:47_

What about `python_callable`?


---

_Review comment by @Lee-W on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:123 on 2024-12-27 05:47_

```suggestion
```

---

_@Lee-W reviewed on 2024-12-27 05:48_

---

_@sunank200 reviewed on 2024-12-30 05:12_

---

_Review comment by @sunank200 on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:123 on 2024-12-30 05:12_

Changed it

---

_@sunank200 reviewed on 2024-12-30 06:14_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:965 on 2024-12-30 06:14_

Changed it: https://github.com/astral-sh/ruff/pull/15144/commits/72dcad72e3f0ea0e6c008341bf5f6ec615d7fdb4

---

_@sunank200 reviewed on 2024-12-30 06:14_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:18 on 2024-12-30 06:14_

I have hard-coded it in Violation now: https://github.com/astral-sh/ruff/pull/15144/commits/72dcad72e3f0ea0e6c008341bf5f6ec615d7fdb4

---

_@uranusjr reviewed on 2024-12-30 07:18_

---

_Review comment by @uranusjr on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:44 on 2024-12-30 07:18_

I don’t think `python_callable` takes the context though? It only accepts things you provide in `self.op_args` and `self.op_kwargs`.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:44 on 2024-12-30 08:12_

I agree that it'll be useful to guard this check by first verifying that the parameter is coming from a function which is decorated with a `@task`.

I think this can be done as a pre-check for context variables by using the `checker.semantic().current_statements()` method to traverse up the AST to find the function definition node and checking whether the function has a `@task` decorator that originates from the `airflow` module.

https://github.com/astral-sh/ruff/blob/9fd4eb8c4cd3ffe4de0540f368a8350a0f920e07/crates/ruff_python_semantic/src/model.rs#L1232-L1239

---

_@dhruvmanila reviewed on 2024-12-30 08:12_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:677 on 2024-12-30 08:13_

I think we'd also need to check where the `context` variable is originating from otherwise, I think, this will raise a violation on all variables that's named "context" and is using a similar access pattern.

---

_@dhruvmanila reviewed on 2024-12-30 08:15_

---

_@dhruvmanila reviewed on 2024-12-30 08:17_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:677 on 2024-12-30 08:17_

i.e., we need to make sure that the definition of `context` variable is the function parameter that's decorated with `@task`.

---

_Label `rule` added by @dhruvmanila on 2024-12-30 08:17_

---

_Label `preview` added by @dhruvmanila on 2024-12-30 08:17_

---

_@Lee-W reviewed on 2024-12-30 14:10_

---

_Review comment by @Lee-W on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:44 on 2024-12-30 14:10_

> I don’t think `python_callable` takes the context though? It only accepts things you provide in `self.op_args` and `self.op_kwargs`.

I though we can still get it in the python_callable? https://airflow.apache.org/docs/apache-airflow/stable/howto/operator/python.html#pythonoperator

---

_@Lee-W reviewed on 2025-01-02 03:06_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:675 on 2025-01-02 03:06_

If `conf` has been added, could you please update the description as well? Thanks!

---

_@uranusjr reviewed on 2025-01-02 07:42_

---

_Review comment by @uranusjr on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:44 on 2025-01-02 07:42_

Hmm OK I didn’t even realise you can do that… yeah in that case it’s probably a good idea to also detect `python_callable` arguments.

---

_Review comment by @jscheffl on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:84 on 2025-01-02 08:54_

For `execution_date` there is actually a replacement - in the docs: https://airflow.apache.org/docs/apache-airflow/stable/templates-ref.html#deprecated-variables - can you add this?

(Same for next_execution_date, prev_execution_date)

---

_@jscheffl reviewed on 2025-01-02 08:54_

---

_@sunank200 reviewed on 2025-01-06 06:30_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:675 on 2025-01-06 06:30_

I have updated the description

---

_@sunank200 reviewed on 2025-01-06 06:30_

---

_Review comment by @sunank200 on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:44 on 2025-01-06 06:30_

I have updated the logic for named argument and function decorated with @task

---

_@sunank200 reviewed on 2025-01-06 06:33_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:677 on 2025-01-06 06:33_

I have changed have added a check for task decorator

---

_@sunank200 reviewed on 2025-01-06 12:28_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:84 on 2025-01-06 12:28_

We have renamed execution_date to logical_date at places but we have removed them as well:https://github.com/apache/airflow/pull/42404

---

_Review requested from @jscheffl by @sunank200 on 2025-01-06 15:32_

---

_Review requested from @dhruvmanila by @sunank200 on 2025-01-06 15:32_

---

_Review requested from @uranusjr by @sunank200 on 2025-01-06 15:32_

---

_Review requested from @Lee-W by @sunank200 on 2025-01-06 15:32_

---

_@uranusjr reviewed on 2025-01-07 08:11_

---

_Review comment by @uranusjr on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:1 on 2025-01-07 08:11_

Do we have a case where a function should _not_ raise a warning?

---

_@uranusjr reviewed on 2025-01-07 08:17_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:99 on 2025-01-07 08:17_

Is it possible to support code like this?

```python
c = get_current_context()
c["execution_date"]
```

We should probably not hard code the variable name.

---

_@uranusjr reviewed on 2025-01-07 08:18_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:332 on 2025-01-07 08:18_

Same, I think we should just check for all names (as long as it’s `**`).

---

_Comment by @dhruvmanila on 2025-01-07 11:55_

Going to focus on reviewing this PR instead of #15240 for now as I think this one supersedes the other one but please correct me if I'm wrong.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:961 on 2025-01-07 12:00_

We should avoid allocating a vector here as we can directly iterate over the statement tree like so:
```rs
for stmt in checker.semantic().current_statements() {
	// ...
}
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:979 on 2025-01-07 12:02_

We can avoid multiple indentation levels by using `let ... else ...` which improves readability like so:

```rs
        let Stmt::FunctionDef(function_def) = stmt else {
            continue;
        };

        if !is_decorated_with(checker, function_def) {
            continue;
        }

        let arguments = extract_task_function_arguments(function_def);

        for deprecated_arg in REMOVED_CONTEXT_KEYS {
            if arguments.contains(&deprecated_arg.to_string()) {
                checker.diagnostics.push(Diagnostic::new(
                    Airflow3Removal {
                        deprecated: deprecated_arg.to_string(),
                        replacement: Replacement::None,
                    },
                    expr.range(),
                ));
                return true;
            }
        }
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:958 on 2025-01-07 12:10_

Can you clarify what this function is suppose to do? I'm confused as to why this function is also looping over the `REMOVED_CONTEXT_ARGS` and adding the diagnostics when the callee also does something similar. Additionally, the name and the return type of this function suggests that it checks for a condition but it's adding diagnostics as well.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:1104 on 2025-01-07 12:12_

Based on the name of the function, did you mean to make this a generic function over any decorator originating from `airflow.decorators.*` ? If not, can you rename this to `has_task_decorator` ?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:986 on 2025-01-07 12:13_

I _think_ we could avoid this allocation but I would like to first understand what does `is_task_context_referenced` function is suppose to do.

---

_@dhruvmanila reviewed on 2025-01-07 12:21_

Thank you for working on this. I've a couple of doubts which I've highlighted in the review comments and https://github.com/astral-sh/ruff/pull/15240#issuecomment-2575072655.

---

_@sunank200 reviewed on 2025-01-08 06:12_

---

_Review comment by @sunank200 on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:1 on 2025-01-08 06:12_

I have added two of them but i can add more.

---

_@sunank200 reviewed on 2025-01-15 06:03_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:979 on 2025-01-15 06:03_

Changed it

---

_@sunank200 reviewed on 2025-01-15 06:07_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:1104 on 2025-01-15 06:07_

Yes, the intent was to check only for the @task decorator. I have renamed this function.

---

_@sunank200 reviewed on 2025-01-15 06:22_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:958 on 2025-01-15 06:22_

`is_task_context_referenced` had multiple responsibilities: check task context usage, extract the task argument and check if they are removed from context variables, add diagnostics and return a boolean. The name of the function was misleading.

I have now renamed to `detect_removed_context_keys_in_task_decorators` and added doc-string as well.

---

_@sunank200 reviewed on 2025-01-15 06:35_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:332 on 2025-01-15 06:35_

added the logic

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:96 on 2025-01-15 07:35_

do we need to make it `pub(crate)` here?

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:99 on 2025-01-15 07:51_

I think we'll need to check 2 cases here.

1. the `context` is from a function argument (e.g., `def func(**context):`)
2. the variable is assigned from `get_current_context`. we probably could do something like https://github.com/astral-sh/ruff/blob/bec8441cf5c0e3870f7e21cc042937d4279268bd/crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs#L265-L267

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:169 on 2025-01-15 07:52_

We probably need to make it configurable. (could be next PR)

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:125 on 2025-01-15 07:52_

same here

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:405 on 2025-01-15 07:53_

we probably will need to check

```python
c = get_current_context()
c.get("execution_date")
```

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:955 on 2025-01-15 07:56_

Is this function only check function with `@task`? we probably need some docstring and make the function name more readable.

---

_@Lee-W reviewed on 2025-01-15 07:57_

---

_@sunank200 reviewed on 2025-01-15 13:24_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:955 on 2025-01-15 13:24_

I have renamed and added the docstring

---

_@sunank200 reviewed on 2025-01-15 13:27_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:96 on 2025-01-15 13:27_

Right, it is not needed as it is used internally. Removed it.

---

_Comment by @dhruvmanila on 2025-01-15 13:29_

Thank you for updating the PR, I plan on looking at it later today.

---

_@sunank200 reviewed on 2025-01-15 17:17_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:169 on 2025-01-15 17:17_

I can create a separate PR for this

---

_@sunank200 reviewed on 2025-01-15 17:17_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:125 on 2025-01-15 17:17_

I can create a separate PR for this

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:1092 on 2025-01-16 09:56_

The arguments are being collected here is being checked against the `REMOVED_CONTEXT_ARGS` array but that array doesn't contain any `**`-prefixed entries.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:1062 on 2025-01-16 10:01_

I think we should just inline this function as it's only used once and avoid allocating a vector here but chaining the arguments that needs to be checked:

```rs
  for param in stmt
      .parameters
      .args
      .iter()
      .map(|param| param.parameter.name.as_str())
      .chain(
          stmt.parameters
              .kwarg
              .as_ref()
              .map(|vararg| vararg.name.as_str()),
      )
  {
	// ...
}
```

---

_@dhruvmanila reviewed on 2025-01-16 10:19_

---

_@dhruvmanila reviewed on 2025-01-16 10:22_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:171 on 2025-01-16 10:22_

Looking at the function usage, the `expr` will always be a `ExprSubscript`, is this leftover from previous iteration?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:139 on 2025-01-16 10:23_

We can directly accept the `ExprSubscript` as the function argument if that's the only expression which is being checked here.

---

_@dhruvmanila reviewed on 2025-01-16 10:23_

---

_@dhruvmanila reviewed on 2025-01-16 10:24_

This is looking good, can you update the PR description to include all the checks that are being done? I'm mainly looking for all the structural matching that's being done here, not specific symbols or variables that's being checked. I'm having a hard time keeping track of them :)

---

_@sunank200 reviewed on 2025-01-17 11:48_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:1092 on 2025-01-17 11:48_

Thats right. Removed it

---

_@sunank200 reviewed on 2025-01-17 12:16_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:171 on 2025-01-17 12:16_

Removed this part completely now as I am taking `ExprSubscript` as argument.

---

_@sunank200 reviewed on 2025-01-17 12:16_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:139 on 2025-01-17 12:16_

changed it as per suggestion

---

_Comment by @dhruvmanila on 2025-01-18 05:19_

Please re-request for review when it's ready :)

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:961 on 2025-01-21 21:15_

removed it

---

_@sunank200 reviewed on 2025-01-21 21:15_

---

_@sunank200 reviewed on 2025-01-21 21:24_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:1062 on 2025-01-21 21:24_

Changed it

---

_@sunank200 reviewed on 2025-01-21 21:24_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:986 on 2025-01-21 21:24_

removed it

---

_@sunank200 reviewed on 2025-01-21 21:25_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:961 on 2025-01-21 21:25_

removed it

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:405 on 2025-01-21 22:01_

Added the check

---

_@sunank200 reviewed on 2025-01-21 22:01_

---

_@sunank200 reviewed on 2025-01-21 22:01_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:99 on 2025-01-21 22:01_

Added the check for both.

---

_Review requested from @dhruvmanila by @sunank200 on 2025-01-21 22:20_

---

_Review requested from @Lee-W by @sunank200 on 2025-01-21 22:20_

---

_Comment by @Lee-W on 2025-01-22 02:40_

> Please re-request for review when it's ready :)

I fix the CI failure. I think it's ready for review now. Thanks!

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302_context.py.snap`:427 on 2025-01-22 02:51_

This seems to be highlighting the wrong range. I think you meant to highlight the parameter `execution_date`?

Can we add more test cases to check functions with multiple parameters which has both deprecated and non-deprecated parameters?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302_context.py.snap`:172 on 2025-01-22 02:52_

Are these related to the changes made in this PR? This is fine, I just want to confirm.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:398 on 2025-01-22 02:55_

I don't think we can use 0 as the default value here. What would happen if it's a positional parameter which is expected to be at position 1 or 2? This will return incorrect argument.

I think we should also add test cases where there are multiple arguments that are deprecated in the same function intermixed with non-deprecated arguments.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:400 on 2025-01-22 03:14_

What's the `**` check is for? I only see `**` being used in parameter in the tests and PR description but this is checking the _call_ function and not the function parameter. Did you meant to look at the definition of `value` and check that instead? If so, I think that will require adding functionality to get the parameter definition for the binding which would be something like:

```rs
fn find_parameter(semantic: &SemanticModel, name: &ast::ExprName) -> Option<ast::AnyParameterRef> {
	let binding_id = semantic.only_binding(name_expr)?;
	let binding = semantic.binding(binding_id);
	let ast::StmtFunctionDef { parameters, .. } = binding.statement(semantic)?.as_function_def_stmt()?;
    parameters
        .iter()
        .find(|parameter| parameter.name().range() == binding.range())
}
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302_context.py.snap`:9 on 2025-01-22 03:32_

This function doesn't have the `@task` decorator but the diagnostics is being raised here, is this correct?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:101 on 2025-01-22 03:33_

Do we need to update the documentation for this function as it's only checking subscript expressions?

---

_@dhruvmanila reviewed on 2025-01-22 03:40_

Thank you for updating the PR description with the patterns that are being checked here, it's helpful as a reference when reviewing.

Regarding 1, 2, 3 pattern, do they need to be in function / method which has the `@task` decorator or can they be in any function / method?

I think we're pretty close to finishing this up, thank you for your patience!

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-01-22 03:41_

---

_Comment by @sunank200 on 2025-01-22 05:56_

> Thank you for updating the PR description with the patterns that are being checked here, it's helpful as a reference when reviewing.
> 
> Regarding 1, 2, 3 pattern, do they need to be in function / method which has the `@task` decorator or can they be in any function / method?
> 
> I think we're pretty close to finishing this up, thank you for your patience!

For pattern 1, 2 and 3 they can be in any function/method.

---

_@sunank200 reviewed on 2025-01-22 06:01_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:101 on 2025-01-22 06:01_

I felt it is easier for someone to read the code logic in this function this way.

---

_@sunank200 reviewed on 2025-01-22 06:03_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302_context.py.snap`:9 on 2025-01-22 06:03_

This is still correct as if the removed variables are part of `**context` in any airflow function, it should raise error.

---

_@dhruvmanila reviewed on 2025-01-22 06:08_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302_context.py.snap`:9 on 2025-01-22 06:08_

Can you briefly describe what is an "airflow function" or provide a reference to it?

---

_@dhruvmanila reviewed on 2025-01-22 06:09_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:101 on 2025-01-22 06:09_

I think we should keep the documentation limited to what the function is doing, we can add this as comments wherever it's relevant.

---

_@sunank200 reviewed on 2025-01-22 06:11_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302_context.py.snap`:172 on 2025-01-22 06:11_

Not related to this PR

---

_@sunank200 reviewed on 2025-01-22 06:24_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:101 on 2025-01-22 06:24_

Changed it now

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302_context.py.snap`:9 on 2025-01-22 06:39_

When I say function in airflow implementation. Let's say there is implementation of operator:
```
from airflow.models.baseoperator import BaseOperator

def log_execution_date(**context):
    # should throw an lint error
    execution_date = context.get("execution_date")
    if execution_date:
        print(f"Warning: 'execution_date' is deprecated. Found value: {execution_date}")
    else:
        print("No 'execution_date' found in context.")

class MyOperator(BaseOperator):
    def __init__(self, my_parameter, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.my_parameter = my_parameter

    def execute(self, context):
        log_execution_date(**context)
```

---

_@sunank200 reviewed on 2025-01-22 06:39_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302_context.py.snap`:427 on 2025-01-22 07:17_

That is right. I have changed the logic to highlight the parameter itself.

I have added more test cases to check functions with multiple parameters that has both deprecated and non-deprecated parameters.

---

_@sunank200 reviewed on 2025-01-22 07:17_

---

_@sunank200 reviewed on 2025-01-22 07:20_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302_context.py.snap`:427 on 2025-01-22 07:20_

https://github.com/astral-sh/ruff/pull/15144/commits/65db31f0e181f8cc469ee9caf7a775dd060b75ff

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:398 on 2025-01-22 14:04_

I think I am also doing a check ` if let Expr::StringLiteral(ExprStringLiteral { value, .. }) = argument ` here as well. 

I have added the test cases for multiple arguments that are deprecated in the same function intermixed with non-deprecated arguments.

---

_@sunank200 reviewed on 2025-01-22 14:04_

---

_@sunank200 reviewed on 2025-01-23 06:34_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:400 on 2025-01-23 06:34_

Added it https://github.com/astral-sh/ruff/pull/15144/commits/d0aff2bbff5c9deb8b9a0bd022d5013d07807bbd

---

_Review requested from @dhruvmanila by @sunank200 on 2025-01-23 06:35_

---

_Review comment by @Lee-W on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:12 on 2025-01-23 06:49_

nit: we can reorder the import to at least fit Python convention

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:100 on 2025-01-23 06:56_

```suggestion
fn check_context_variable(checker: &mut Checker, subscript: &ExprSubscript) {
```

the naming convention (at least in this module) has been changed

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:99 on 2025-01-23 06:57_

It would be nice to have an example like other functions

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:104 on 2025-01-23 06:59_

What happens when we encounter `**kwargs`? e.g.,

```python
def func(**kwargs):
    kwargs["conf"]
```

will it still be true?

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:384 on 2025-01-23 07:10_

what is gcc?

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:384 on 2025-01-23 07:42_

this could be a function and could be reused in `check_context_variable`

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:364 on 2025-01-23 07:50_

The function name might not be accurate 🤔 

```
/// ```python
/// from airflow.decorators import task
///
/// @task
/// def another_task(execution_date, **kwargs):
///     # 'execution_date' is removed in Airflow 3.0
///     pass
/// ```
```

This is not context_get

---

_@Lee-W reviewed on 2025-01-23 07:50_

---

_@dhruvmanila reviewed on 2025-01-23 08:07_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302_context.py.snap`:9 on 2025-01-23 08:07_

Thanks for providing an example. So, in this case, the `log_execution_date` is an Airflow function, right?

---

_@Lee-W reviewed on 2025-01-23 08:09_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302_context.py.snap`:9 on 2025-01-23 08:09_

It's a user-defined function, but when `context` is passed through `BaseOperator.execute` and then to `log_execution_date`, it should not access removed keys

---

_@sunank200 reviewed on 2025-01-23 08:35_

---

_Review comment by @sunank200 on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:12 on 2025-01-23 08:35_

changed it.

---

_@sunank200 reviewed on 2025-01-23 08:37_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:99 on 2025-01-23 08:37_

It was suggest to remove it here: https://github.com/astral-sh/ruff/pull/15144#discussion_r1924747014

---

_@sunank200 reviewed on 2025-01-23 08:43_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:364 on 2025-01-23 08:43_

changed it to `check_removed_context_keys_usage`

---

_Comment by @dhruvmanila on 2025-01-23 08:45_

For pattern 4, we should use a separate entrypoint which directly checks the function definition instead of trying to find the function definition from the expression otherwise we'll end up with duplicate diagnostics like in the following example:
```py
from airflow.decorators import task


@task
def some_task(execution_date, conf, **kwargs):
    kwargs.get("execution_date")
    kwargs.get("conf")
```

<details><summary>Output of running Ruff on this PR:</summary>
<p>

```
/Users/dhruv/playground/ruff/src/AIR302.py:5:15: AIR302 `execution_date` is removed in Airflow 3.0
  |
4 | @task
5 | def some_task(execution_date, conf, **kwargs):
  |               ^^^^^^^^^^^^^^ AIR302
6 |     kwargs.get("execution_date")
7 |     kwargs.get("conf")
  |

/Users/dhruv/playground/ruff/src/AIR302.py:5:15: AIR302 `execution_date` is removed in Airflow 3.0
  |
4 | @task
5 | def some_task(execution_date, conf, **kwargs):
  |               ^^^^^^^^^^^^^^ AIR302
6 |     kwargs.get("execution_date")
7 |     kwargs.get("conf")
  |

/Users/dhruv/playground/ruff/src/AIR302.py:5:31: AIR302 `conf` is removed in Airflow 3.0
  |
4 | @task
5 | def some_task(execution_date, conf, **kwargs):
  |                               ^^^^ AIR302
6 |     kwargs.get("execution_date")
7 |     kwargs.get("conf")
  |

/Users/dhruv/playground/ruff/src/AIR302.py:5:31: AIR302 `conf` is removed in Airflow 3.0
  |
4 | @task
5 | def some_task(execution_date, conf, **kwargs):
  |                               ^^^^ AIR302
6 |     kwargs.get("execution_date")
7 |     kwargs.get("conf")
  |

/Users/dhruv/playground/ruff/src/AIR302.py:6:16: AIR302 `execution_date` is removed in Airflow 3.0
  |
4 | @task
5 | def some_task(execution_date, conf, **kwargs):
6 |     kwargs.get("execution_date")
  |                ^^^^^^^^^^^^^^^^ AIR302
7 |     kwargs.get("conf")
  |

/Users/dhruv/playground/ruff/src/AIR302.py:7:16: AIR302 `conf` is removed in Airflow 3.0
  |
5 | def some_task(execution_date, conf, **kwargs):
6 |     kwargs.get("execution_date")
7 |     kwargs.get("conf")
  |                ^^^^^^ AIR302
  |

Found 6 errors.
```

</p>
</details>

Can you make this change? I tried pushing it but I'm unable to do so, do you have "allow edits to maintainers off" (https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/allowing-changes-to-a-pull-request-branch-created-from-a-fork) ? Here's a patch for that:

```diff
commit c76e15a45d61a42d95a4fbaf23aceae9134652db
Author: Dhruv Manilawala <dhruvmanila@gmail.com>
Date:   Thu Jan 23 14:13:29 2025 +0530

    Check context parameters directly from function definition

diff --git a/crates/ruff_linter/src/checkers/ast/analyze/statement.rs b/crates/ruff_linter/src/checkers/ast/analyze/statement.rs
index d5f92b9f2..7f64be4bd 100644
--- a/crates/ruff_linter/src/checkers/ast/analyze/statement.rs
+++ b/crates/ruff_linter/src/checkers/ast/analyze/statement.rs
@@ -376,6 +376,9 @@ pub(crate) fn statement(stmt: &Stmt, checker: &mut Checker) {
             if checker.enabled(Rule::PytestParameterWithDefaultArgument) {
                 flake8_pytest_style::rules::parameter_with_default_argument(checker, function_def);
             }
+            if checker.enabled(Rule::Airflow3Removal) {
+                airflow::rules::removed_in_3_function_def(checker, function_def);
+            }
         }
         Stmt::Return(_) => {
             if checker.enabled(Rule::ReturnOutsideFunction) {
diff --git a/crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs b/crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs
index b08f8986b..f36dd29db 100644
--- a/crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs
+++ b/crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs
@@ -168,6 +168,36 @@ pub(crate) fn removed_in_3(checker: &mut Checker, expr: &Expr) {
     }
 }
 
+/// AIR302
+pub(crate) fn removed_in_3_function_def(checker: &mut Checker, function_def: &StmtFunctionDef) {
+    if !checker.semantic().seen_module(Modules::AIRFLOW) {
+        return;
+    }
+
+    if !is_airflow_task(function_def, checker.semantic()) {
+        return;
+    }
+
+    for param in function_def
+        .parameters
+        .posonlyargs
+        .iter()
+        .chain(function_def.parameters.args.iter())
+        .chain(function_def.parameters.kwonlyargs.iter())
+    {
+        let param_name = param.parameter.name.as_str();
+        if REMOVED_CONTEXT_KEYS.contains(&param_name) {
+            checker.diagnostics.push(Diagnostic::new(
+                Airflow3Removal {
+                    deprecated: param_name.to_string(),
+                    replacement: Replacement::None,
+                },
+                param.parameter.name.range(),
+            ));
+        }
+    }
+}
+
 #[derive(Debug, Eq, PartialEq)]
 enum Replacement {
     None,
@@ -398,37 +428,6 @@ fn check_context_get(checker: &mut Checker, call_expr: &ExprCall) {
     if attr.as_str() != "get" {
         return;
     }
-    let function_def = {
-        let mut parents = checker.semantic().current_statements();
-        parents.find_map(|stmt| {
-            if let Stmt::FunctionDef(func_def) = stmt {
-                Some(func_def.clone())
-            } else {
-                None
-            }
-        })
-    };
-
-    if let Some(func_def) = function_def {
-        for param in func_def
-            .parameters
-            .posonlyargs
-            .iter()
-            .chain(func_def.parameters.args.iter())
-            .chain(func_def.parameters.kwonlyargs.iter())
-        {
-            let param_name = param.parameter.name.as_str();
-            if REMOVED_CONTEXT_KEYS.contains(&param_name) {
-                checker.diagnostics.push(Diagnostic::new(
-                    Airflow3Removal {
-                        deprecated: param_name.to_string(),
-                        replacement: Replacement::None,
-                    },
-                    param.parameter.name.range(),
-                ));
-            }
-        }
-    }
 
     for removed_key in REMOVED_CONTEXT_KEYS {
         if let Some(argument) = call_expr.arguments.find_argument_value(removed_key, 0) {
@@ -1087,3 +1086,14 @@ fn is_airflow_builtin_or_provider(segments: &[&str], module: &str, symbol_suffix
         _ => false,
     }
 }
+
+/// Returns `true` if the given function is decorated with `@airflow.decorators.task`.
+fn is_airflow_task(function_def: &StmtFunctionDef, semantic: &SemanticModel) -> bool {
+    function_def.decorator_list.iter().any(|decorator| {
+        semantic
+            .resolve_qualified_name(map_callable(&decorator.expression))
+            .is_some_and(|qualified_name| {
+                matches!(qualified_name.segments(), ["airflow", "decorators", "task"])
+            })
+    })
+}
```

---

_@sunank200 reviewed on 2025-01-23 08:46_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:384 on 2025-01-23 08:46_

It is `get_current_context`. I have changed it to `is_assigned_from_get_current_context`.

---

_@sunank200 reviewed on 2025-01-23 08:46_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:384 on 2025-01-23 08:46_

I think it is just used in check_removed_context_keys_usage function

---

_@sunank200 reviewed on 2025-01-23 08:47_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:100 on 2025-01-23 08:47_

changed it

---

_@dhruvmanila reviewed on 2025-01-23 08:52_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:398 on 2025-01-23 08:52_

Oh, sorry, this is for the call expression. I think I confused this with the function parameter check.

Does `context.get` allow keyword argument? The `find_arguments_value` will first check if there's a keyword with the given name (as indicated by `removed_key` variable in this case) and then check for positional argument. I'm assuming that `context.get(conf="conf")` is not allowed and only `context.get("conf")` is allowed which is why I think we should directly use `find_positional` instead.

(I've marked this as unresolved to make sure I don't forget to check this)

---

_Comment by @sunank200 on 2025-01-23 09:01_

> use a separate entrypoint which directly checks the function definition

@dhruvmanila changed it.

---

_@sunank200 reviewed on 2025-01-23 09:11_

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:104 on 2025-01-23 09:11_

It doesn't throw a lint error for this.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:100 on 2025-01-23 09:42_

Do we need to check for `context.get` pattern in here as well? I think that's not being checked as per my local testing. For example, the following will not raise any diagnostics:

```python
from airflow.models.baseoperator import BaseOperator


class CustomOperator(BaseOperator):
    def execute(self, context):
        execution_date = context.get("execution_date")
        next_ds = context.get("next_ds")
```

Now, as per:

> 2\. The `execute` function of a BaseOperator subclass (...)

It seems that it's not _any_ method but rather a specific method and only when it's inherited by `BaseOperator`. I don't think we're checking this. Is that ok?

---

_@dhruvmanila reviewed on 2025-01-23 09:48_

I have one last doubt but otherwise this looks good.

For a `@task` decorated functions, we check for both `var.get(...)` and `var[...]` pattern but there are other places where we only check subscript expressions. Is that expected? Should those places also check for `var.get(...)` pattern? If yes, are there additional requirements in those places similar to how there's a requirement for a `@task` decorator?

---

_@Lee-W reviewed on 2025-01-23 10:03_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:398 on 2025-01-23 10:03_

I think we can only do `context.get("conf")`

---

_@Lee-W reviewed on 2025-01-23 10:17_

---

_Review comment by @Lee-W on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:100 on 2025-01-23 10:17_

1. I think we'll need to check this
2. any function that is called in `BaseOperator.execute`. It's possible to have false positive now but the chance are low I think



```e.g.,

def any_func(context):
    context.get("execution_date")

class CustomOperator(BaseOperator):
    def execute(self, context):
        any_func(context)
```

---

_@sunank200 reviewed on 2025-01-23 16:40_

---

_Review comment by @sunank200 on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:100 on 2025-01-23 16:40_

1. Added the change

---

_Comment by @sunank200 on 2025-01-23 16:43_

> I have one last doubt but otherwise this looks good.
> 
> For a `@task` decorated functions, we check for both `var.get(...)` and `var[...]` pattern but there are other places where we only check subscript expressions. Is that expected? Should those places also check for `var.get(...)` pattern? If yes, are there additional requirements in those places similar to how there's a requirement for a `@task` decorator?

@dhruvmanila I have added a check for both `var.get(...)` and `var[...]` pattern at all places.

---

_Review comment by @sunank200 on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:398 on 2025-01-23 16:53_

Using `find_positional` directly.

---

_@sunank200 reviewed on 2025-01-23 16:53_

---

_@dhruvmanila reviewed on 2025-01-24 05:51_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:100 on 2025-01-24 05:51_

> 2\. any function that is called in `BaseOperator.execute`. It's possible to have false positive now but the chance are low I think

Yeah, I don't think we're checking this. I'm fine merging this as is for now but one solution might be to collect all the functions that occur in this context and then defer checking of them. But, this will make it a bit complex and might not be worth it.

---

_@dhruvmanila reviewed on 2025-01-24 05:53_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_context.py`:73 on 2025-01-24 05:53_

I don't see this in the snapshot. Should we remove this test case?

(I'll do it in a follow-up PR.)

---

_@dhruvmanila approved on 2025-01-24 05:54_

Thank you! Welcome to Ruff! 🥳

---

_Renamed from "[airflow] Add lint rule to show error for removed context variables in airflow" to "[`airflow`] Update `AIR302` to check for deprecated context keys" by @dhruvmanila on 2025-01-24 05:54_

---

_Merged by @dhruvmanila on 2025-01-24 05:55_

---

_Closed by @dhruvmanila on 2025-01-24 05:55_

---

_@dhruvmanila reviewed on 2025-01-24 06:10_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:146 on 2025-01-24 06:10_

Can this variable name be anything other than `context`?

---

_@dhruvmanila reviewed on 2025-01-24 06:11_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:417 on 2025-01-24 06:11_

Similarly, can this parameter be anything other than `context` or `kwargs`?

---

_@dhruvmanila reviewed on 2025-01-24 06:40_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:197 on 2025-01-24 06:40_

This doesn't really make sense as both function is going to perform the same check except that the first one is behind an additional check of `@task` function. I'm not sure why this is present.

---

_@dhruvmanila reviewed on 2025-01-24 06:42_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:418 on 2025-01-24 06:42_

The parameter name won't really contain `**` at the start. You can see that in the playground: https://play.ruff.rs/f7051131-c6c0-49a4-8b91-e74c37ae8088

---

_Comment by @dhruvmanila on 2025-01-24 06:49_

Sorry for prematurely merging this PR. I see a lot of inconsistencies from which I've pointed out some of them above. I'm going to revert some of the changes and only keep the ones to only perform the check in `@task`-decorated functions. Please create a follow-up PR to perform this check in other contexts like the `Operator.execute` method.

---

_Branch deleted on 2025-02-01 00:02_

---
