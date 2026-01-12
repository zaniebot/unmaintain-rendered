```yaml
number: 11471
title: "[`pydoclint`] Implement `docstring-missing-exception` and `docstring-extraneous-exception` (`DOC501`, `DOC502`)"
type: pull_request
state: merged
author: augustelalande
labels:
  - rule
  - docstring
  - preview
assignees: []
merged: true
base: main
head: darglint
created_at: 2024-05-19T20:43:34Z
updated_at: 2024-07-21T16:13:24Z
url: https://github.com/astral-sh/ruff/pull/11471
synced_at: 2026-01-12T15:55:38Z
```

# [`pydoclint`] Implement `docstring-missing-exception` and `docstring-extraneous-exception` (`DOC501`, `DOC502`)

---

_@augustelalande_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

These are the first rules implemented as part of #458, but I plan to implement more.

Specifically, this implements `docstring-missing-exception` which checks for raised exceptions not documented in the docstring, and `docstring-extraneous-exception` which checks for exceptions in the docstring not present in the body.

## Test Plan

Test fixtures added for both google and numpy style.


---

_Comment by @codspeed-hq[bot] on 2024-05-19 21:05_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/augustelalande:darglint)

### Merging #11471 will **degrade performances by 97.48%**

<sub>Comparing <code>augustelalande:darglint</code> (9a6edd4) with <code>augustelalande:darglint</code> (0434606)</sub>



### Summary

`❌ 3` regressions
`✅ 30` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/augustelalande:darglint)._

### Benchmarks breakdown

|     | Benchmark | `augustelalande:darglint` | `augustelalande:darglint` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `red_knot_check_file[cold]` | 346.4 µs | 13,748.4 µs | -97.48% |
| ❌ | `red_knot_check_file[incremental]` | 95 µs | 422.7 µs | -77.53% |
| ❌ | `red_knot_check_file[without_parse]` | 260.5 µs | 6,345.5 µs | -95.89% |


---

_Comment by @github-actions[bot] on 2024-05-19 21:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2239 -0 violations, +0 -0 fixes in 3 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1844 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/__init__.py#L47'>airflow/api/__init__.py:47:15:</a> DAR401 Raised exception `AirflowException` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/client/json_client.py#L55'>airflow/api/client/json_client.py:55:19:</a> DAR401 Raised exception `OSError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/delete_dag.py#L62'>airflow/api/common/delete_dag.py:62:15:</a> DAR401 Raised exception `AirflowException` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/delete_dag.py#L65'>airflow/api/common/delete_dag.py:65:15:</a> DAR401 Raised exception `DagNotFound` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/experimental/__init__.py#L37'>airflow/api/common/experimental/__init__.py:37:15:</a> DAR401 Raised exception `DagNotFound` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/experimental/__init__.py#L43'>airflow/api/common/experimental/__init__.py:43:15:</a> DAR401 Raised exception `DagNotFound` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/experimental/__init__.py#L46'>airflow/api/common/experimental/__init__.py:46:15:</a> DAR401 Raised exception `TaskNotFound` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/experimental/__init__.py#L55'>airflow/api/common/experimental/__init__.py:55:15:</a> DAR401 Raised exception `DagRunNotFound` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/experimental/get_code.py#L44'>airflow/api/common/experimental/get_code.py:44:15:</a> DAR401 Raised exception `AirflowException` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/experimental/get_task_instance.py#L44'>airflow/api/common/experimental/get_task_instance.py:44:15:</a> DAR401 Raised exception `TaskInstanceNotFound` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/experimental/get_task_instance.py#L48'>airflow/api/common/experimental/get_task_instance.py:48:11:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/experimental/pool.py#L40'>airflow/api/common/experimental/pool.py:40:15:</a> DAR401 Raised exception `AirflowBadRequest` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/experimental/pool.py#L44'>airflow/api/common/experimental/pool.py:44:15:</a> DAR401 Raised exception `PoolNotFound` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/experimental/pool.py#L61'>airflow/api/common/experimental/pool.py:61:15:</a> DAR401 Raised exception `AirflowBadRequest` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/experimental/pool.py#L66'>airflow/api/common/experimental/pool.py:66:15:</a> DAR401 Raised exception `AirflowBadRequest` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/experimental/pool.py#L71'>airflow/api/common/experimental/pool.py:71:15:</a> DAR401 Raised exception `AirflowBadRequest` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/experimental/pool.py#L92'>airflow/api/common/experimental/pool.py:92:15:</a> DAR401 Raised exception `AirflowBadRequest` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/experimental/pool.py#L95'>airflow/api/common/experimental/pool.py:95:15:</a> DAR401 Raised exception `AirflowBadRequest` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/experimental/pool.py#L99'>airflow/api/common/experimental/pool.py:99:15:</a> DAR401 Raised exception `PoolNotFound` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/mark_tasks.py#L123'>airflow/api/common/mark_tasks.py:123:15:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/mark_tasks.py#L126'>airflow/api/common/mark_tasks.py:126:15:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/mark_tasks.py#L130'>airflow/api/common/mark_tasks.py:130:15:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/mark_tasks.py#L133'>airflow/api/common/mark_tasks.py:133:15:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/mark_tasks.py#L138'>airflow/api/common/mark_tasks.py:138:15:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/mark_tasks.py#L300'>airflow/api/common/mark_tasks.py:300:15:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/mark_tasks.py#L333'>airflow/api/common/mark_tasks.py:333:15:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/mark_tasks.py#L400'>airflow/api/common/mark_tasks.py:400:19:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/mark_tasks.py#L403'>airflow/api/common/mark_tasks.py:403:19:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/mark_tasks.py#L406'>airflow/api/common/mark_tasks.py:406:15:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/mark_tasks.py#L453'>airflow/api/common/mark_tasks.py:453:19:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/mark_tasks.py#L456'>airflow/api/common/mark_tasks.py:456:19:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/mark_tasks.py#L460'>airflow/api/common/mark_tasks.py:460:15:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/mark_tasks.py#L544'>airflow/api/common/mark_tasks.py:544:19:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/mark_tasks.py#L547'>airflow/api/common/mark_tasks.py:547:19:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/mark_tasks.py#L550'>airflow/api/common/mark_tasks.py:550:15:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/trigger_dag.py#L123'>airflow/api/common/trigger_dag.py:123:15:</a> DAR401 Raised exception `DagNotFound` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/trigger_dag.py#L56'>airflow/api/common/trigger_dag.py:56:15:</a> DAR401 Raised exception `DagNotFound` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/trigger_dag.py#L61'>airflow/api/common/trigger_dag.py:61:15:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/trigger_dag.py#L69'>airflow/api/common/trigger_dag.py:69:19:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api/common/trigger_dag.py#L82'>airflow/api/common/trigger_dag.py:82:15:</a> DAR401 Raised exception `DagRunAlreadyExists` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/27b3a22e341468855c4ef368015ad946a59aa2e3/airflow/api_connexion/endpoints/config_endpoint.py#L87'>airflow/api_connexion/endpoints/config_endpoint.py:87:19:</a> DAR401 Raised exception `NotFound` missing from docstring
... 1803 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+189 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L168'>src/bokeh/application/application.py:168:19:</a> DAR401 Raised exception `RuntimeError` missing from docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L94'>src/bokeh/application/handlers/code_runner.py:94:19:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L142'>src/bokeh/application/handlers/directory.py:142:19:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L153'>src/bokeh/application/handlers/directory.py:153:19:</a> DAR401 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L213'>src/bokeh/client/connection.py:213:19:</a> DAR401 Raised exception `RuntimeError` missing from docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L215'>src/bokeh/client/connection.py:215:19:</a> DAR401 Raised exception `RuntimeError` missing from docstring
... 182 additional changes omitted for rule DAR401
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/resources.py#L165'>src/bokeh/resources.py:165:1:</a> DAR402 KeyError not explicitly raised.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/tile_providers.py#L182'>src/bokeh/tile_providers.py:182:1:</a> DAR402 ValueError not explicitly raised.
... 181 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+206 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/6af748fa774fe381def42ec946ef0d73f24e71ae/analytics/lib/fixtures.py#L57'>analytics/lib/fixtures.py:57:15:</a> DAR401 Raised exception `AssertionError` missing from docstring
+ <a href='https://github.com/zulip/zulip/blob/6af748fa774fe381def42ec946ef0d73f24e71ae/analytics/lib/fixtures.py#L59'>analytics/lib/fixtures.py:59:15:</a> DAR401 Raised exception `AssertionError` missing from docstring
+ <a href='https://github.com/zulip/zulip/blob/6af748fa774fe381def42ec946ef0d73f24e71ae/confirmation/models.py#L102'>confirmation/models.py:102:15:</a> DAR401 Raised exception `ConfirmationKeyError` missing from docstring
+ <a href='https://github.com/zulip/zulip/blob/6af748fa774fe381def42ec946ef0d73f24e71ae/confirmation/models.py#L105'>confirmation/models.py:105:15:</a> DAR401 Raised exception `ConfirmationKeyError` missing from docstring
+ <a href='https://github.com/zulip/zulip/blob/6af748fa774fe381def42ec946ef0d73f24e71ae/confirmation/models.py#L115'>confirmation/models.py:115:15:</a> DAR401 Raised exception `ConfirmationKeyError` missing from docstring
+ <a href='https://github.com/zulip/zulip/blob/6af748fa774fe381def42ec946ef0d73f24e71ae/confirmation/models.py#L96'>confirmation/models.py:96:15:</a> DAR401 Raised exception `ConfirmationKeyError` missing from docstring
+ <a href='https://github.com/zulip/zulip/blob/6af748fa774fe381def42ec946ef0d73f24e71ae/corporate/lib/stripe.py#L1160'>corporate/lib/stripe.py:1160:19:</a> DAR401 Raised exception `BillingError` missing from docstring
+ <a href='https://github.com/zulip/zulip/blob/6af748fa774fe381def42ec946ef0d73f24e71ae/corporate/lib/stripe.py#L1209'>corporate/lib/stripe.py:1209:23:</a> DAR401 Raised exception `StripeCardError` missing from docstring
+ <a href='https://github.com/zulip/zulip/blob/6af748fa774fe381def42ec946ef0d73f24e71ae/corporate/views/remote_billing_page.py#L203'>corporate/views/remote_billing_page.py:203:15:</a> DAR401 Raised exception `AssertionError` missing from docstring
+ <a href='https://github.com/zulip/zulip/blob/6af748fa774fe381def42ec946ef0d73f24e71ae/corporate/views/remote_billing_page.py#L218'>corporate/views/remote_billing_page.py:218:15:</a> DAR401 Raised exception `JsonableError` missing from docstring
... 196 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DAR401 | 2237 | 2237 | 0 | 0 | 0 |
| DAR402 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `rule` added by @charliermarsh on 2024-05-20 00:38_

---

_Label `docstring` added by @charliermarsh on 2024-05-20 00:38_

---

_Comment by @cr1901 on 2024-05-22 23:11_

If this PR is accepted, will this preclude `pydoclint` (`darglint` successor but not really backwards-compat) support in the future? While `pydoclint` is the tool I personally use, it seems since yesterday, there are few thumbs-up asking [about using `pydoclint`](https://github.com/astral-sh/ruff/issues/458#issuecomment-2122773827) as a base instead.

---

_Comment by @augustelalande on 2024-05-23 00:25_

I'm not opposed to using pydocstyle instead, but I'll wait until I get a review from the ruff team to change anything.

---

_@charliermarsh approved on 2024-07-17 16:49_

Nice, this looks great. I'll make a few tweaks and merge today.

---

_Comment by @augustelalande on 2024-07-17 23:57_

@charliermarsh where do you stand on darglint vs pydoclint as mentioned above

---

_Comment by @charliermarsh on 2024-07-18 00:56_

Honestly, finding it hard to have a strong opinion on it... What do you think?

Absent other opinions, let's run with `pydoclint` given that it's actively developed? (And re-code these as `DOC501` and `DOC502`?)

---

_Comment by @augustelalande on 2024-07-18 00:58_

Sounds reasonable to me

---

_Renamed from "[`darglint`] Implement `docstring-missing-exception` and `docstring-extraneous-exception` (`DAR401`, `DAR402`)" to "[`pydoclint`] Implement `docstring-missing-exception` and `docstring-extraneous-exception` (`DOC501`, `DOC502`)" by @augustelalande on 2024-07-18 15:19_

---

_@charliermarsh reviewed on 2024-07-18 22:24_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:62 on 2024-07-18 22:24_

What if this is like, `raise foo.FasterThanLightError from exc`? Would / should the docstring contain `FasterThanLightError` or `foo.FasterThanLightError`?

---

_@charliermarsh reviewed on 2024-07-18 22:25_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydocstyle/helpers.rs`:99 on 2024-07-18 22:25_

I'd probably replace this `break` control flow with an early `return`, since this method just returns `section_contexts` anyway.

---

_@charliermarsh reviewed on 2024-07-18 22:26_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:303 on 2024-07-18 22:26_

This implies function-only, right?

---

_@charliermarsh reviewed on 2024-07-18 22:27_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:172 on 2024-07-18 22:27_

Can you document this and the above to give an example of the expected structure that it's parsing?

---

_@charliermarsh reviewed on 2024-07-18 22:27_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:160 on 2024-07-18 22:27_

Could this return `Vec<&str>`?

---

_@charliermarsh reviewed on 2024-07-18 22:27_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:225 on 2024-07-18 22:27_

Per my comment above, I wonder if we need to match on attributes here too. What do you think?

---

_@charliermarsh reviewed on 2024-07-18 22:28_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:228 on 2024-07-18 22:28_

I wonder if we instead want to store the resolved name... Like if the user does:

```python
from foo import Bar as Baz

def func():
   """Raises: Bar"""
   raise Baz()
```

Should that be considered ok, or no?

---

_@augustelalande reviewed on 2024-07-19 02:22_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:303 on 2024-07-19 02:22_

Yes. At least for now since exceptions can't be raised by classes AFAIK.

---

_@augustelalande reviewed on 2024-07-19 02:40_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:62 on 2024-07-19 02:40_

Hard to say. I found nothing in the google style guide or numpy style guide. Per your other comments, I'll try to make it agnostic.

---

_@augustelalande reviewed on 2024-07-19 03:13_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:228 on 2024-07-19 03:13_

How would you resolve the qualified name from the docstring? Say we have 
```python
import foo

def func():
   """Raises: Bar"""
   raise foo.Bar()
```
Can we even resolve that this is correct?

---

_@augustelalande reviewed on 2024-07-19 05:47_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:228 on 2024-07-19 05:47_

Ok so I made it so that any suffix of the fully qualified name is allowed in the docstring

---

_@T-256 reviewed on 2024-07-19 11:10_

---

_Review comment by @T-256 on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:75 on 2024-07-19 11:10_

```suggestion
/// ## What it does
```

---

_@ddelange reviewed on 2024-07-19 13:14_

---

_Review comment by @ddelange on `crates/ruff_linter/src/checkers/ast/analyze/definitions.rs`:328 on 2024-07-19 13:14_

1. will the pydoclint rules get their own settings section, or will pydoclint re-use the existing [`pydocstyle.convention`](https://docs.astral.sh/ruff/faq/#does-ruff-support-numpy-or-google-style-docstrings) setting?
2. if DOC gets a `pydoclint` settings section, will the user have to duplicate `convention` directive?
3. will `convention` be a required config, or what will be the default?
4. will the pydoclint rules support `pep257` convention just like pydocstyle?


---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/analyze/definitions.rs`:328 on 2024-07-19 13:18_

I think we'll just reuse the `convention` setting from `pydocstyle` for now and document it as such in the `pydoclint` rules. Eventually, we'll want to recategorize all of these under a single `docstrings` section, so it'll be unified at some point in the future.


---

_@charliermarsh reviewed on 2024-07-19 13:18_

---

_@augustelalande reviewed on 2024-07-19 14:21_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/checkers/ast/analyze/definitions.rs`:328 on 2024-07-19 14:21_

4. Yes we will eventually add support for `pep257` and `sphinx` style docstrings

---

_Label `preview` added by @charliermarsh on 2024-07-20 19:38_

---

_@charliermarsh reviewed on 2024-07-20 19:40_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:147 on 2024-07-20 19:40_

I changed this to return `None` when there's no section, so we didn't have to `unwrap` the `raised_exceptions_range`. Feel free to revert in future PRs if that gets in the way, though I think something _like_ that is useful to get rid of the unwrap.

---

_Merged by @charliermarsh on 2024-07-20 19:41_

---

_Closed by @charliermarsh on 2024-07-20 19:41_

---

_Comment by @T-256 on 2024-07-20 22:14_

Do we need to update title and description of #458 to keep tracking future implementations for this plugin?

---

_Branch deleted on 2024-07-21 03:30_

---

_Comment by @augustelalande on 2024-07-21 16:13_

I'm thinking to open a new issue to track `pydoclint` and supersede #458

---
