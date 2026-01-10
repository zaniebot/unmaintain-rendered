```yaml
number: 12581
title: Improve consistency between linter rules in determining whether a function is property
type: pull_request
state: merged
author: AlexWaygood
labels:
  - linter
assignees: []
merged: true
base: main
head: alex/is-property
created_at: 2024-07-30T13:39:37Z
updated_at: 2024-07-30T16:54:47Z
url: https://github.com/astral-sh/ruff/pull/12581
synced_at: 2026-01-10T21:47:02Z
```

# Improve consistency between linter rules in determining whether a function is property

---

_Pull request opened by @AlexWaygood on 2024-07-30 13:39_

## Summary

We have a function in `ruff_python_semantic` for detecting whether a function is a property or not. However, there are various linter rules where we've forgotten to use it. That means that our linter rules are internally inconsistent about whether they consider methods decorated with `@functools.cached_property` to be a property method, for example. This PR fixes that.

## Test Plan

`cargo test -p ruff_linter --lib`

---

_Label `linter` added by @AlexWaygood on 2024-07-30 13:39_

---

_@AlexWaygood reviewed on 2024-07-30 13:40_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/property_with_parameters.rs`:1 on 2024-07-30 13:40_

The changes to this file increase the scope of this rule. We could therefore make the changes to this rule a preview-only change. However, I don't see it as a significant increase in scope: I doubt there will be many new hits on user code as a result of this change. The error that the rule is trying to catch is pretty uncommon, anyway.

---

_Comment by @github-actions[bot] on 2024-07-30 13:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -14 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -13 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/models/taskinstance.py#L1944'>airflow/models/taskinstance.py:1944:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/models/taskinstance.py#L2008'>airflow/models/taskinstance.py:2008:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/providers/alibaba/cloud/sensors/oss_key.py#L102'>airflow/providers/alibaba/cloud/sensors/oss_key.py:102:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/providers/amazon/aws/utils/mixins.py#L161'>airflow/providers/amazon/aws/utils/mixins.py:161:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/providers/amazon/aws/utils/mixins.py#L171'>airflow/providers/amazon/aws/utils/mixins.py:171:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/providers/apache/spark/operators/spark_sql.py#L103'>airflow/providers/apache/spark/operators/spark_sql.py:103:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/providers/google/cloud/operators/mlengine.py#L1490'>airflow/providers/google/cloud/operators/mlengine.py:1490:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/providers/google/cloud/operators/mlengine.py#L1499'>airflow/providers/google/cloud/operators/mlengine.py:1499:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/providers/google/cloud/operators/mlengine.py#L1509'>airflow/providers/google/cloud/operators/mlengine.py:1509:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/providers/google/cloud/operators/vertex_ai/auto_ml.py#L651'>airflow/providers/google/cloud/operators/vertex_ai/auto_ml.py:651:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/providers/google/cloud/operators/vertex_ai/custom_job.py#L1642'>airflow/providers/google/cloud/operators/vertex_ai/custom_job.py:1642:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/providers/google/cloud/operators/vertex_ai/custom_job.py#L1652'>airflow/providers/google/cloud/operators/vertex_ai/custom_job.py:1652:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/providers/google/common/hooks/base_google.py#L466'>airflow/providers/google/common/hooks/base_google.py:466:9:</a> DOC201 `return` is not documented in docstring
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/c3702be9d4e5d603041c87097a1b9c38f345386a/superset/viz.py#L673'>superset/viz.py:673:9:</a> DOC201 `return` is not documented in docstring
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DOC201 | 14 | 0 | 14 | 0 | 0 |

</p>
</details>




---

_Comment by @AlexWaygood on 2024-07-30 13:58_

The ecosystem check has a lot of hits going away for `DOC201`, which tries to skip functions it considers to be properties. Previously it only considered a function to be a property if the _last_ decorator on that function was `@property` (or similar); now it considers a function to be a property if _any_ decorator on that function is `@property` (or similar). I think the new behaviour is correct.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:487 on 2024-07-30 16:28_

I think I'm reviewing the PRs in the wrong order. This is now behind your new iterator and no longer requires collecting?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/definition.rs`:164 on 2024-07-30 16:29_

I wonder if it's worth having this method in `definition` if it only ever is called from `pylint` rules. I would move it upwards next to the lint rule.

---

_@MichaReiser approved on 2024-07-30 16:30_

There's also https://github.com/astral-sh/ruff/blob/7a4419a2a52b7f9e4eda4afb4bd1fe770d59c0ae/crates/ruff_python_semantic/src/analyze/visibility.rs#L66-L83

and they both seem slightly different...

---

_@AlexWaygood reviewed on 2024-07-30 16:38_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:487 on 2024-07-30 16:38_

> I think I'm reviewing the PRs in the wrong order.

You are, yes :-) This is the first PR in the stack, and the other two PRs are based on top of this branch.

But as I mentioned in https://github.com/astral-sh/ruff/pull/12582#discussion_r1697209176, I think we still need to collect ~always even with my new iterator, because the common pattern seems to be to do something like this:

```rs
for decorator in decorators {
    if extra_properties.any(|property| property == decorator) {
        return true;
    }
}
```

If `extra_properties` is an iterator there, it could be exhausted after the first iteration of the outer `for` loop

---

_@AlexWaygood reviewed on 2024-07-30 16:40_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/definition.rs`:164 on 2024-07-30 16:40_

I'd prefer to have it here, so that it can be easily reused by other rules that we add in the future

---

_Comment by @AlexWaygood on 2024-07-30 16:41_

> There's also
> 
> https://github.com/astral-sh/ruff/blob/7a4419a2a52b7f9e4eda4afb4bd1fe770d59c0ae/crates/ruff_python_semantic/src/analyze/visibility.rs#L66-L83
> 
> and they both seem slightly different...

That's the function I'm switching the rules over to using in this PR!

---

_Merged by @AlexWaygood on 2024-07-30 16:42_

---

_Closed by @AlexWaygood on 2024-07-30 16:42_

---

_Branch deleted on 2024-07-30 16:42_

---

_@MichaReiser reviewed on 2024-07-30 16:51_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:487 on 2024-07-30 16:51_

We can `Clone` the iterator to avoid this

---

_@AlexWaygood reviewed on 2024-07-30 16:54_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:487 on 2024-07-30 16:54_

Oh, good point.

---
