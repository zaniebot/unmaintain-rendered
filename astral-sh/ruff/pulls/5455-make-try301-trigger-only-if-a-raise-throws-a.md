```yaml
number: 5455
title: "Make TRY301 trigger only if a `raise` throws a caught exception "
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: 5246_try301_identical_rule
created_at: 2023-06-30T23:24:40Z
updated_at: 2023-07-10T14:10:32Z
url: https://github.com/astral-sh/ruff/pull/5455
synced_at: 2026-01-12T03:36:55Z
```

# Make TRY301 trigger only if a `raise` throws a caught exception 

---

_Pull request opened by @evanrittenhouse on 2023-06-30 23:24_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #5246. We generate a hash set of all exception IDs caught by the `try` statement, then check that the inner `raise` actually raises a caught exception.

## Test Plan

<!-- How was it tested? -->
Added a new test, `cargo t`.

---

_Renamed from "Make TRY301 trigger if a `raise` throws the same exception that the s…" to "Make TRY301 trigger only if a `raise` throws a caught exception " by @evanrittenhouse on 2023-06-30 23:26_

---

_Comment by @github-actions[bot] on 2023-06-30 23:37_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -44, 0 error(s))

<details><summary>airflow (+0, -21)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/main/airflow/api_connexion/endpoints/pool_endpoint.py#L97'>airflow/api_connexion/endpoints/pool_endpoint.py:97:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/jobs/backfill_job_runner.py#L598'>airflow/jobs/backfill_job_runner.py:598:29:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/jobs/backfill_job_runner.py#L927'>airflow/jobs/backfill_job_runner.py:927:25:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/providers/apache/hive/hooks/hive.py#L181'>airflow/providers/apache/hive/hooks/hive.py:181:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/providers/apache/spark/hooks/spark_jdbc.py#L162'>airflow/providers/apache/spark/hooks/spark_jdbc.py:162:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/providers/apache/spark/hooks/spark_jdbc.py#L164'>airflow/providers/apache/spark/hooks/spark_jdbc.py:164:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/providers/apache/spark/hooks/spark_submit.py#L203'>airflow/providers/apache/spark/hooks/spark_submit.py:203:21:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/providers/apache/spark/hooks/spark_submit.py#L210'>airflow/providers/apache/spark/hooks/spark_submit.py:210:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/providers/arangodb/hooks/arangodb.py#L107'>airflow/providers/arangodb/hooks/arangodb.py:107:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/providers/common/sql/operators/sql.py#L1179'>airflow/providers/common/sql/operators/sql.py:1179:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/providers/databricks/hooks/databricks_base.py#L268'>airflow/providers/databricks/hooks/databricks_base.py:268:25:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/providers/databricks/hooks/databricks_base.py#L335'>airflow/providers/databricks/hooks/databricks_base.py:335:25:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/providers/databricks/hooks/databricks_base.py#L405'>airflow/providers/databricks/hooks/databricks_base.py:405:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/providers/databricks/hooks/databricks_base.py#L422'>airflow/providers/databricks/hooks/databricks_base.py:422:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/providers/google/cloud/hooks/mlengine.py#L63'>airflow/providers/google/cloud/hooks/mlengine.py:63:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/providers/google/cloud/operators/dataproc.py#L2320'>airflow/providers/google/cloud/operators/dataproc.py:2320:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/providers/hashicorp/_internal_client/vault_client.py#L372'>airflow/providers/hashicorp/_internal_client/vault_client.py:372:21:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/providers/jenkins/operators/jenkins_job_trigger.py#L214'>airflow/providers/jenkins/operators/jenkins_job_trigger.py:214:25:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/providers/jenkins/operators/jenkins_job_trigger.py#L56'>airflow/providers/jenkins/operators/jenkins_job_trigger.py:56:13:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/providers/microsoft/azure/operators/container_instances.py#L270'>airflow/providers/microsoft/azure/operators/container_instances.py:270:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/apache/airflow/blob/main/airflow/utils/process_utils.py#L320'>airflow/utils/process_utils.py:320:17:</a> TRY301 Abstract `raise` to an inner function
</pre>

</p>
</details>
<details><summary>bokeh (+0, -3)</summary>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/branch-3.2/src/bokeh/core/property/dataspec.py#L497'>src/bokeh/core/property/dataspec.py:497:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/bokeh/bokeh/blob/branch-3.2/src/bokeh/core/property/dataspec.py#L512'>src/bokeh/core/property/dataspec.py:512:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/bokeh/bokeh/blob/branch-3.2/src/bokeh/core/property/dataspec.py#L526'>src/bokeh/core/property/dataspec.py:526:17:</a> TRY301 Abstract `raise` to an inner function
</pre>

</p>
</details>
<details><summary>zulip (+0, -20)</summary>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/main/tools/lib/template_parser.py#L159'>tools/lib/template_parser.py:159:21:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/tools/lib/template_parser.py#L231'>tools/lib/template_parser.py:231:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/zerver/decorator.py#L704'>zerver/decorator.py:704:13:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/zerver/lib/narrow.py#L409'>zerver/lib/narrow.py:409:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/zerver/lib/send_email.py#L286'>zerver/lib/send_email.py:286:13:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/zerver/lib/streams.py#L459'>zerver/lib/streams.py:459:9:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/zerver/lib/user_groups.py#L36'>zerver/lib/user_groups.py:36:13:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/zerver/lib/user_groups.py#L43'>zerver/lib/user_groups.py:43:13:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/zerver/lib/validator.py#L155'>zerver/lib/validator.py:155:13:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/zerver/tests/test_openapi.py#L295'>zerver/tests/test_openapi.py:295:13:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/zerver/tests/test_timeout.py#L29'>zerver/tests/test_timeout.py:29:13:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/zerver/tests/test_timeout.py#L38'>zerver/tests/test_timeout.py:38:13:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/zerver/tests/test_timeout.py#L51'>zerver/tests/test_timeout.py:51:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/zerver/views/custom_profile_fields.py#L66'>zerver/views/custom_profile_fields.py:66:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/zerver/views/user_settings.py#L273'>zerver/views/user_settings.py:273:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/zerver/views/users.py#L510'>zerver/views/users.py:510:9:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/zerver/views/users.py#L712'>zerver/views/users.py:712:9:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/zerver/webhooks/ifttt/view.py#L30'>zerver/webhooks/ifttt/view.py:30:17:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/zerver/webhooks/ifttt/view.py#L33'>zerver/webhooks/ifttt/view.py:33:13:</a> TRY301 Abstract `raise` to an inner function
- <a href='https://github.com/zulip/zulip/blob/main/zilencer/views.py#L65'>zilencer/views.py:65:13:</a> TRY301 Abstract `raise` to an inner function
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| TRY301 | 44 | 0 | 44 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04      9.2±0.20ms     4.4 MB/sec    1.00      8.9±0.26ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.0±0.07ms     8.3 MB/sec    1.00  1962.4±68.50µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    223.0±8.04µs    13.2 MB/sec    1.00    223.2±6.21µs    13.2 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.16ms     5.9 MB/sec    1.01      4.4±0.16ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.02     15.5±0.51ms     2.6 MB/sec    1.00     15.2±0.43ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.9±0.13ms     4.3 MB/sec    1.00      3.8±0.11ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   481.2±16.10µs     6.1 MB/sec    1.05   505.2±10.12µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.23ms     3.8 MB/sec    1.02      6.8±0.20ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.22ms     5.4 MB/sec    1.01      7.7±0.26ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1691.8±53.24µs     9.8 MB/sec    1.00  1691.7±53.43µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.3±8.36µs    15.7 MB/sec    1.05    197.2±5.70µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.13ms     7.5 MB/sec    1.02      3.5±0.13ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.4±0.12ms     4.8 MB/sec    1.00      8.3±0.09ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.01  1831.3±35.88µs     9.1 MB/sec    1.00  1821.2±29.09µs     9.1 MB/sec
formatter/numpy/globals.py                 1.00    207.0±7.16µs    14.3 MB/sec    1.01    208.7±8.76µs    14.1 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.07ms     6.3 MB/sec    1.01      4.1±0.08ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.4±0.18ms     2.8 MB/sec    1.02     14.7±0.20ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.04ms     4.5 MB/sec    1.02      3.8±0.05ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    455.0±8.76µs     6.5 MB/sec    1.01    459.0±8.25µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.09ms     4.0 MB/sec    1.03      6.5±0.12ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.07ms     5.6 MB/sec    1.07      7.8±0.08ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1547.3±35.57µs    10.8 MB/sec    1.06  1643.0±23.50µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.5±3.48µs    16.3 MB/sec    1.05    190.5±4.71µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.04ms     7.8 MB/sec    1.06      3.5±0.05ms     7.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @MichaReiser on 2023-07-03 08:34_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/tryceratops/rules/raise_within_try.rs`:114 on 2023-07-03 08:36_

Should we just ignore the `exception` if `get_function_name` returns `None` or is there a case where we want to lookup `""` in `handler_ids`?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/tryceratops/rules/raise_within_try.rs`:116 on 2023-07-03 08:36_

Nit: Does the following work?
```suggestion
        let Stmt::Raise(ast::StmtRaise { exc: Some(exception), .. }) = stmt else {
            continue;
        };
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/tryceratops/rules/raise_within_try.rs`:117 on 2023-07-03 08:37_

Do we need to check if `Exception` is a builtin type?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/tryceratops/rules/raise_within_try.rs`:105 on 2023-07-03 08:39_

Nit: It could be faster to use `Vec` here, considering we'll only have very few entries. We could also potentially make this lazy to avoid allocating the vector for try statements that have no `raise` statement.

---

_@MichaReiser reviewed on 2023-07-03 08:39_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/tryceratops/rules/raise_within_try.rs`:105 on 2023-07-03 12:57_

@MichaReiser Sorry - under what circumstances is a `Vec` faster than an `iter()`? Just for future reference

---

_@evanrittenhouse reviewed on 2023-07-03 12:57_

---

_@evanrittenhouse reviewed on 2023-07-03 12:58_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/tryceratops/rules/raise_within_try.rs`:117 on 2023-07-03 12:58_

Do we have a way to do that? I thought it'd suffice to just go off of name, but if we have a more robust way happy to change it

---

_@evanrittenhouse reviewed on 2023-07-03 13:00_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/tryceratops/rules/raise_within_try.rs`:114 on 2023-07-03 13:00_

Yeah, could also just ignore it - there's a case where `raise` with no arguments raises the last handled arg, but I guess the `exc` argument there is `None` rather than `""`. Sorry, should've checked that

---

_@MichaReiser reviewed on 2023-07-03 14:43_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/tryceratops/rules/raise_within_try.rs`:105 on 2023-07-03 14:43_

Oh, sorry. Iterating over a `Vec` can be faster than looking up a set if you have very few items and if comparing the elements is cheap. This is mainly because sets have an additional level of indirection. It first needs to find the bucket, then compare the element in that bucket. We probably also don't bother about duplicates here. If we have a duplicate, so what... 


---

_@evanrittenhouse reviewed on 2023-07-03 20:43_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/tryceratops/rules/raise_within_try.rs`:117 on 2023-07-03 20:43_

@MichaReiser just in case this slipped through 

---

_@MichaReiser reviewed on 2023-07-04 06:25_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/tryceratops/rules/raise_within_try.rs`:117 on 2023-07-04 06:25_

Thanks for the pining. I, indeed, missed this message. 

Can we use [is_builtin](https://github.com/astral-sh/ruff/blob/7ac9e0252e3c278078e19a7053671fdc845986af/crates/ruff_python_semantic/src/model.rs#L239-L241) for this?

---

_@evanrittenhouse reviewed on 2023-07-04 18:01_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/tryceratops/rules/raise_within_try.rs`:105 on 2023-07-04 18:01_

Interesting. I was using a set for the O(1) lookup. Is there a rule of thumb for when that `Vec` iteration is quicker?

---

_Comment by @evanrittenhouse on 2023-07-06 22:15_

@MichaReiser bumping for re-review :)

---

_@MichaReiser approved on 2023-07-07 09:41_

---

_Comment by @MichaReiser on 2023-07-07 09:42_

I let @charliermarsh do the final review as our chief of rules ;)

---

_@charliermarsh reviewed on 2023-07-08 19:06_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/tryceratops/rules/raise_within_try.rs`:97 on 2023-07-08 19:06_

I think we should use `ComparableExpr` here -- it lets us compare expressions as value types (i.e., it ignores locations when doing the comparison). IIRC it's like `ComparableExpr::from(expr)` to cast it.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/tryceratops/rules/raise_within_try.rs`:120 on 2023-07-08 19:09_

Alternatively:

```rust
if handler_exprs.contains(ComparableExpr::from(map_callable(exception)) || handler_exprs.iter().any(|expr| checker.resolve().resolve_call_path(expr).map_or(false, |call_path| matches!(call_path.as_slice(), ["", "Exception" | "BaseException"]) {
  ...
}
```

`map_callable` will take the `func` if the expression is a call, or return the expression otherwise (which will also ensure we handle `raise MyException` (no parens required technically)). Using `ComparableExpr` ensures we handle attributes (`raise foo.Bar()`).


---

_@charliermarsh reviewed on 2023-07-08 19:09_

---

_@charliermarsh requested changes on 2023-07-08 19:09_

Couple nits inline, thanks!

---

_@evanrittenhouse reviewed on 2023-07-09 13:11_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/tryceratops/rules/raise_within_try.rs`:97 on 2023-07-09 13:11_

Oh, didn't know about that. Thanks!

---

_Review requested from @charliermarsh by @evanrittenhouse on 2023-07-09 16:56_

---

_@charliermarsh reviewed on 2023-07-10 01:47_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/tryceratops/rules/raise_within_try.rs`:99 on 2023-07-10 01:47_

Do we need this condition? I kind of suspect we can omit it entirely, to handle cases like:

```
try:
  raise foo.Bar()
except foo.Bar:
  pass
```

Right now, this only seems to support names, but `ComparableExpr` should make it such that we can support any expression.

---

_@evanrittenhouse reviewed on 2023-07-10 13:08_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/tryceratops/rules/raise_within_try.rs`:99 on 2023-07-10 13:08_

@charliermarsh I think you're right. Thanks for the pointers - need to get more familiar with the AST side of things. 

I've given you edit permissions, I'm going to be pretty busy for the next month or so and am not sure I'll be able to monitor this. Feel free to make any changes you need. Thanks again

---

_Merged by @charliermarsh on 2023-07-10 14:00_

---

_Closed by @charliermarsh on 2023-07-10 14:00_

---

_Comment by @charliermarsh on 2023-07-10 14:00_

Thanks!

---

_Branch deleted on 2023-07-10 14:10_

---
