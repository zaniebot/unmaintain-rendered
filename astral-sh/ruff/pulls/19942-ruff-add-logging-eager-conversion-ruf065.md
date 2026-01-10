```yaml
number: 19942
title: "[`ruff`] Add `logging-eager-conversion` (`RUF065`)"
type: pull_request
state: merged
author: GDYendell
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: logging-str-preformat
created_at: 2025-08-16T18:59:49Z
updated_at: 2025-09-19T20:43:44Z
url: https://github.com/astral-sh/ruff/pull/19942
synced_at: 2026-01-10T17:40:28Z
```

# [`ruff`] Add `logging-eager-conversion` (`RUF065`)

---

_Pull request opened by @GDYendell on 2025-08-16 18:59_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #12734

I have started with simply checking if any arguments that are providing extra values to the log message are calls to `str` or `repr`, as suggested in the linked issue. There was a concern that this could cause false positives and the check should be more explicit. I am happy to look into that if I have some further examples to work with.

If this is the accepted solution then there are more cases to add to the test and it should possibly also do test for the same behavior via the `extra` keyword.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

I have added a new test case and python file to flake8_logging_format with examples of this anti-pattern.

<!-- How was it tested? -->


---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:178 on 2025-08-18 13:36_

Could we move this into the `is_rule_enabled` branch below? It shouldn't be too expensive but that narrows the scope at least too.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/violations.rs`:337 on 2025-08-18 13:39_

```suggestion
/// Checks for pre-formatting of arguments to `logging` calls.
```

---

_Label `rule` added by @ntBre on 2025-08-18 13:39_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/violations.rs`:349 on 2025-08-18 13:43_

tiny nit: I believe all of the instances of "parameters" here should be "arguments." I think I also prefer "`logging` calls" over to "logging statements," as I suggested above too.

```suggestion
/// Arguments to `logging` calls will be formatted as strings automatically, so it
/// is unnecessary and less efficient to eagerly format the arguments before passing
/// them in.
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/violations.rs`:350 on 2025-08-18 13:45_

Hmm, it made sense to me at first to follow the G004 docs, but now I'm not sure if the `extra` discussion is very applicable. I think in this rule, the user is already using the `extra`/positional passing suggested by G004, so we may be able to omit any `extra` discussion. Does that sound right to you, or am I missing something?

---

_Review comment by @ntBre on `crates/ruff_linter/src/codes.rs`:964 on 2025-08-18 13:47_

New rules need to be added in `RuleGroup::Preview`.

---

_Comment by @github-actions[bot] on 2025-08-18 13:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+131 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+41 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/70f0fada065bca75817def1dd7227fd8da812725/airflow-core/src/airflow/dag_processing/bundles/manager.py#L144'>airflow-core/src/airflow/dag_processing/bundles/manager.py:144:66:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/airflow/blob/70f0fada065bca75817def1dd7227fd8da812725/airflow-core/src/airflow/models/dagbag.py#L134'>airflow-core/src/airflow/models/dagbag.py:134:49:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/airflow/blob/70f0fada065bca75817def1dd7227fd8da812725/airflow-core/src/airflow/utils/email.py#L261'>airflow-core/src/airflow/utils/email.py:261:52:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/airflow/blob/70f0fada065bca75817def1dd7227fd8da812725/airflow-core/tests/integration/otel/dags/otel_test_dag.py#L48'>airflow-core/tests/integration/otel/dags/otel_test_dag.py:48:54:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/airflow/blob/70f0fada065bca75817def1dd7227fd8da812725/providers/amazon/src/airflow/providers/amazon/aws/hooks/dms.py#L243'>providers/amazon/src/airflow/providers/amazon/aws/hooks/dms.py:243:78:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/airflow/blob/70f0fada065bca75817def1dd7227fd8da812725/providers/amazon/src/airflow/providers/amazon/aws/hooks/dms.py#L315'>providers/amazon/src/airflow/providers/amazon/aws/hooks/dms.py:315:71:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/airflow/blob/70f0fada065bca75817def1dd7227fd8da812725/providers/amazon/src/airflow/providers/amazon/aws/hooks/dms.py#L369'>providers/amazon/src/airflow/providers/amazon/aws/hooks/dms.py:369:68:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/airflow/blob/70f0fada065bca75817def1dd7227fd8da812725/providers/amazon/src/airflow/providers/amazon/aws/transfers/s3_to_dynamodb.py#L179'>providers/amazon/src/airflow/providers/amazon/aws/transfers/s3_to_dynamodb.py:179:92:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/airflow/blob/70f0fada065bca75817def1dd7227fd8da812725/providers/apache/tinkerpop/src/airflow/providers/apache/tinkerpop/hooks/gremlin.py#L150'>providers/apache/tinkerpop/src/airflow/providers/apache/tinkerpop/hooks/gremlin.py:150:75:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/airflow/blob/70f0fada065bca75817def1dd7227fd8da812725/providers/arangodb/src/airflow/providers/arangodb/hooks/arangodb.py#L156'>providers/arangodb/src/airflow/providers/arangodb/hooks/arangodb.py:156:62:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/airflow/blob/70f0fada065bca75817def1dd7227fd8da812725/providers/arangodb/src/airflow/providers/arangodb/hooks/arangodb.py#L167'>providers/arangodb/src/airflow/providers/arangodb/hooks/arangodb.py:167:62:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/airflow/blob/70f0fada065bca75817def1dd7227fd8da812725/providers/arangodb/src/airflow/providers/arangodb/hooks/arangodb.py#L178'>providers/arangodb/src/airflow/providers/arangodb/hooks/arangodb.py:178:63:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/airflow/blob/70f0fada065bca75817def1dd7227fd8da812725/providers/arangodb/src/airflow/providers/arangodb/hooks/arangodb.py#L189'>providers/arangodb/src/airflow/providers/arangodb/hooks/arangodb.py:189:62:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/airflow/blob/70f0fada065bca75817def1dd7227fd8da812725/providers/databricks/src/airflow/providers/databricks/operators/databricks_sql.py#L391'>providers/databricks/src/airflow/providers/databricks/operators/databricks_sql.py:391:98:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/airflow/blob/70f0fada065bca75817def1dd7227fd8da812725/providers/databricks/src/airflow/providers/databricks/operators/databricks_sql.py#L426'>providers/databricks/src/airflow/providers/databricks/operators/databricks_sql.py:426:100:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
... 26 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+82 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/annotation_layers/annotations/api.py#L306'>superset/annotation_layers/annotations/api.py:306:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/annotation_layers/annotations/api.py#L380'>superset/annotation_layers/annotations/api.py:380:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/annotation_layers/annotations/api.py#L434'>superset/annotation_layers/annotations/api.py:434:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/annotation_layers/api.py#L158'>superset/annotation_layers/api.py:158:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/annotation_layers/api.py#L222'>superset/annotation_layers/api.py:222:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/annotation_layers/api.py#L293'>superset/annotation_layers/api.py:293:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/charts/api.py#L374'>superset/charts/api.py:374:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/charts/api.py#L450'>superset/charts/api.py:450:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/charts/api.py#L507'>superset/charts/api.py:507:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/charts/api.py#L785'>superset/charts/api.py:785:70:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/commands/dashboard/importers/v0.py#L79'>superset/commands/dashboard/importers/v0.py:79:36:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/commands/report/execute.py#L613'>superset/commands/report/execute.py:613:76:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/commands/report/log_prune.py#L54'>superset/commands/report/log_prune.py:54:25:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/commands/report/log_prune.py#L55'>superset/commands/report/log_prune.py:55:25:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/daos/dataset.py#L57'>superset/daos/dataset.py:57:62:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/daos/dataset.py#L92'>superset/daos/dataset.py:92:68:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/dashboards/api.py#L1324'>superset/dashboards/api.py:1324:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/dashboards/api.py#L604'>superset/dashboards/api.py:604:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/dashboards/api.py#L690'>superset/dashboards/api.py:690:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/dashboards/api.py#L769'>superset/dashboards/api.py:769:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/dashboards/api.py#L851'>superset/dashboards/api.py:851:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/dashboards/api.py#L907'>superset/dashboards/api.py:907:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/databases/api.py#L2048'>superset/databases/api.py:2048:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/databases/api.py#L2056'>superset/databases/api.py:2056:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/databases/api.py#L482'>superset/databases/api.py:482:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/databases/api.py#L567'>superset/databases/api.py:567:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/databases/api.py#L624'>superset/databases/api.py:624:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/datasets/api.py#L1026'>superset/datasets/api.py:1026:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/datasets/api.py#L371'>superset/datasets/api.py:371:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/datasets/api.py#L455'>superset/datasets/api.py:455:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/apache/superset/blob/1e4bc6ee78c8553f5e4fa65aefcac9a5a38ef239/superset/datasets/api.py#L511'>superset/datasets/api.py:511:17:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
... 51 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/203e8be1dfbb509aa4157876937faaa20fd1e6ad/corporate/lib/stripe.py#L5373'>corporate/lib/stripe.py:5373:13:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/zulip/zulip/blob/203e8be1dfbb509aa4157876937faaa20fd1e6ad/zerver/lib/email_mirror_server.py#L68'>zerver/lib/email_mirror_server.py:68:75:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/zulip/zulip/blob/203e8be1dfbb509aa4157876937faaa20fd1e6ad/zproject/backends.py#L2068'>zproject/backends.py:2068:13:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/zulip/zulip/blob/203e8be1dfbb509aa4157876937faaa20fd1e6ad/zproject/backends.py#L2844'>zproject/backends.py:2844:54:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/zulip/zulip/blob/203e8be1dfbb509aa4157876937faaa20fd1e6ad/zproject/backends.py#L2966'>zproject/backends.py:2966:64:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/zulip/zulip/blob/203e8be1dfbb509aa4157876937faaa20fd1e6ad/zproject/backends.py#L2988'>zproject/backends.py:2988:65:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/zulip/zulip/blob/203e8be1dfbb509aa4157876937faaa20fd1e6ad/zproject/backends.py#L3004'>zproject/backends.py:3004:65:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
+ <a href='https://github.com/zulip/zulip/blob/203e8be1dfbb509aa4157876937faaa20fd1e6ad/zproject/backends.py#L3076'>zproject/backends.py:3076:77:</a> RUF065 Unnecessary `str()` conversion when formatting with `%s`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF065 | 131 | 131 | 0 | 0 | 0 |

</p>
</details>




---

_@ntBre reviewed on 2025-08-18 14:00_

Thank you! This looks good to me overall, I mostly had small nit comments.

Otherwise, I'm not totally sure if we can make this a G rule. I see an upstream issue was opened last August, at the same time as the Ruff issue (https://github.com/globality-corp/flake8-logging-format/issues/79). There hasn't been a response there specifically, but there has been activity in the repo in the last 3 months. The rule fits really nicely here with the other G rule implementations, as well as thematically, but we also don't want to run into rule conflicts down the line. @MichaReiser do we need to make this a RUF rule to be safe?

I'm also not totally sold on the name. What do you think about `LoggingEagerConversion`? Just an idea, I'm not sure I love that either.

Finally, would you mind looking through the ecosystem results? The simple implementation here is very appealing, but we'll want to make sure there aren't any false positives that might warrant parsing and cross-referencing the formatting arguments, as Charlie mentioned on the issue.

---

_Review comment by @GDYendell on `crates/ruff_linter/src/rules/flake8_logging_format/violations.rs`:350 on 2025-08-18 16:42_

I was wondering about this and whether this should check should look at `extra`, but
given the user could be constructing a dict inline I think this would be excessive. I
would say this rule should not consider `extra` at all and we should remove any mention
of it here.

---

_@GDYendell reviewed on 2025-08-18 16:42_

---

_Comment by @GDYendell on 2025-08-18 17:13_

Thanks for the review! I added a couple of fixup commits to address your comments.

> I'm also not totally sold on the name. What do you think about LoggingEagerConversion? Just an idea, I'm not sure I love that either.

I wasn't sure what to call it and chose the name in the hope that someone would come up with something better  :sweat_smile:

I prefer `LoggingEagerConversion`. It could also be `LoggingEagerFormat` or `LoggingEagerCast`?

> Finally, would you mind looking through the ecosystem results? The simple implementation here is very appealing, but we'll want to make sure there aren't any false positives that might warrant parsing and cross-referencing the formatting arguments, as Charlie mentioned on the issue.

Will do - this is a really cool CI helper! I have had a quick look and there are some casts of exceptions, which possibly do more the logging formatter does.

---

_Comment by @GDYendell on 2025-08-18 19:52_

I have had a look through the examples in the affected ecosystem packages and I don't see any issues. As a summary:
- Exceptions in except blocks - implements `__str__` and it is less work to just pass `exc_info=True` to the logging call with better results
- Path objects - implements `__str__`
- os.getpid() and other integers - implements `__str__`
- apache superset Slice.to_json() - not sure where the implementation is, by imagine this returns a str
- packaging Version - implements `__str__`

I didn't see any hits that aren't explicitly calling str or repr,  so I think those are all correct matches.

---

_@ntBre reviewed on 2025-08-19 17:41_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/violations.rs`:350 on 2025-08-19 17:41_

It looks like the first paragraph of `## Why is this bad?` still talks about `extra`. Should we remove that part too? I think it would be fine to lead with the second paragraph: `Arguments to logging calls...`

---

_Label `preview` added by @ntBre on 2025-08-19 17:44_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:184 on 2025-08-19 17:50_

One thing I forgot to mention before, we should use `SemanticModel::match_builtin_expr` here. That will handle (rare) cases where `str` or `repr` has been overwritten:

https://github.com/astral-sh/ruff/blob/c6dcfe36d060f693d771e4ae3f6156527b67c816/crates/ruff_linter/src/rules/flake8_pyi/rules/str_or_repr_defined_in_stub.rs#L81-L83

We don't want to return in this case, but that's an example usage.

We could also add a small test for that behavior.

---

_@ntBre reviewed on 2025-08-19 18:29_

Thanks! I had one more small suggestion, and then I did find a potential issue in the ecosystem check. A case like this is currently flagged:

```py
import logging
try:
    1/0
except Exception as e:
    logging.error("error: %s", e)
```

That's no problem, the output is the same after dropping the `str` call:

```pycon
>>> try:
...     1/0
... except Exception as e:
...     logging.error("error: %s", str(e))
...
ERROR:root:error: division by zero
>>> try:
...     1/0
... except Exception as e:
...     logging.error("error: %s", e)
...
ERROR:root:error: division by zero
>>>
```

However, mixing `%s` and `repr`, _does_ change the result:

```pycon
>>> try:
...     1/0
... except Exception as e:
...     logging.error("error: %s", repr(e))
...
ERROR:root:error: ZeroDivisionError('division by zero')
>>> try:
...     1/0
... except Exception as e:
...     logging.error("error: %s", e)
...
ERROR:root:error: division by zero
```

I saw this in this langchain [example](https://github.com/langchain-ai/langchain/blob/02d6b9106baa9f23f195abfd5d6d948d85eee29b/libs/core/langchain_core/callbacks/manager.py#L2493-L2494) from the ecosystem check. That's not a super big problem given the current diagnostic message, which doesn't directly tell you to drop the `repr` call, but we'd at least need to add an example to the docs where `%r` is used.

However, the reverse combination of `%r` and `str` might actually be out of scope for the rule because it changes the output in a way that I don't think we can easily capture (abbreviated example to make comparison easier):

```py
logging.error("error: %r", e)  # ERROR:root:error: ZeroDivisionError('division by zero')
logging.error("error: %r", str(e))  # ERROR:root:error: 'division by zero'
logging.error("error: %s", e)  # ERROR:root:error: division by zero
```

Note the quotes in `%r`/`str` compared to just `%s`.

I think that means we need to go ahead and do the format string parsing and comparison with the positional arguments that Charlie mentioned in the issue. We would need to do that for a future autofix anyway, although it's fine to leave a fix itself for a follow-up PR. What do you think?

This might be less of a big deal than I'm imagining, but flagging cases where users need to change `"%r", str(...)` to `"'%r'", ...` feels like it could be confusing and less desirable compared to cases where you just drop a `str` or `repr` call.

Otherwise I think we just need to decide on the name and rule code!

---

_@GDYendell reviewed on 2025-08-19 18:39_

---

_Review comment by @GDYendell on `crates/ruff_linter/src/rules/flake8_logging_format/violations.rs`:350 on 2025-08-19 18:39_

Ah yeah it does... I am happy to remove that, but I do think all of these rules having the same opening paragraph is helpful for the reader. I find it odd that it suggests that `extra` is the primary way to pass additional values to logging statements with positional arguments as an alternative when - in my experience - the latter is much more common. It might be worth rewording this to suggest positional args so that they can continue to share the same opening paragraph and mention `extra` as the alternative for the rules that is relevant to.

What do you think?

---

_@ntBre reviewed on 2025-08-19 18:51_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/violations.rs`:350 on 2025-08-19 18:51_

I can see what you mean, but I wouldn't really want to update the other rule docs in this PR. I guess I'm starting to see this rule as conceptually fairly different from G001-G004 anyway, so I don't mind if the docs are different. There's still a chance it will need to be a RUF rule too.

---

_Comment by @GDYendell on 2025-08-19 19:09_

> A case like this is currently flagged:
> ```python
> import logging
> try:
>     1/0
> except Exception as e:
>     logging.error("error: %s", e)
> ```

Is your first snippet here supposed to have `str(e)` rather than `e`, or is it actually flagging a case where it doesn't use `str` or `repr`?

> However, mixing %s and repr, does change the result:
> ```python
> try:
> ...     1/0
> ... except Exception as e:
> ...     logging.error("error: %s", repr(e))
> ...
> ERROR:root:error: ZeroDivisionError('division by zero')
> ```

Ah that is interesting, I hadn't appreciated that. This is a tricky one then because the user might prefer that output. They would want to use `logging.error("error: %r", e)` to get the same, so we need to change the suggestion depending on whether `str` or `repr` was used. If you are happy to leave that until another PR, for now maybe it would be useful to say something like "use `%s` to format with `str` or `%r` to format with `repr`".

The `%r` & `str()` case is an odd one - would that really be intentional? I am not sure what the suggestion should be because you can't get the same output with just a format string. It is relying on casting to a `str` and then formatting the `repr` of a `str`, which wraps it in quotes. This doesn't give the same output: 

```python
try:
   1/0
except Exception as e:
   logging.error("error: %r", str(e))

ERROR:root:error: 'division by zero'
```

```python
try:
   1/0
except Exception as e:
   logging.error("error: '%r'", e)

ERROR:root:error: 'ZeroDivisionError('division by zero')'
```

---

_@GDYendell reviewed on 2025-08-19 19:11_

---

_Review comment by @GDYendell on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:184 on 2025-08-19 19:11_

Ah good one. I will update it.

---

_Comment by @ntBre on 2025-08-19 19:43_

> Is your first snippet here supposed to have `str(e)` rather than `e`, or is it actually flagging a case where it doesn't use `str` or `repr`?

Ah yes, you're right, sorry!

> Ah that is interesting, I hadn't appreciated that. This is a tricky one then because the user might prefer that output. They would want to use `logging.error("error: %r", e)` to get the same, so we need to change the suggestion depending on whether `str` or `repr` was used. If you are happy to leave that until another PR, for now maybe it would be useful to say something like "use `%s` to format with `str` or `%r` to format with `repr`".

I think I'd prefer to go ahead and give a tailored message for each case we know about, if it's not too much more work. I ran into a somewhat analogous case for another rule recently where we got reports about the confusing, generic help message. This looks like an example of similar parsing in another rule that doesn't look too bad:

https://github.com/astral-sh/ruff/blob/58efd19f11af5e24eed665e4381d5b9e0bdf130e/crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs#L388-L407

> The `%r` & `str()` case is an odd one - would that really be intentional? I am not sure what the suggestion should be because you can't get the same output with just a format string. It is relying on casting to a `str` and then formatting the `repr` of a `str`, which wraps it in quotes. This doesn't give the same output:

Oops, good catch again. Yeah, we might want to avoid a diagnostic in this case. It seems like kind of a roundabout way to wrap something in quotes, but I still think it's intentional. We could always expand the rule later, but I'd lean more towards definite true positives like `%s` paired with `str` for a first draft.

I also remembered that there's a vaguely similar rule for f-strings: [explicit-f-string-type-conversion (RUF010)](https://docs.astral.sh/ruff/rules/explicit-f-string-type-conversion/#explicit-f-string-type-conversion-ruf010). This replaces calls like `f"{repr(a)}"` with `f"{a!r}"`. Most notably, it applies to `str`, `repr`, and `ascii`. I think we could apply this rule to `ascii` and `%a` as well, but I think that's definitely the least commonly used of the three. Just another idea.


---

_Comment by @GDYendell on 2025-08-19 21:40_

OK I think I see how to do that. Just to confirm, we want to iterate over the placeholders in the format string with the corresponding values and only flag `%s` with `str()` and `%r` with `repr()` as issues, ignoring the two inverted cases for now?

---

_Comment by @ntBre on 2025-08-20 18:51_

> OK I think I see how to do that. Just to confirm, we want to iterate over the placeholders in the format string with the corresponding values and only flag `%s` with `str()` and `%r` with `repr()` as issues, ignoring the two inverted cases for now?

Yep, that's exactly what I had in mind!

---

_Comment by @GDYendell on 2025-08-20 23:39_

It now only flags %s with str() and %r with repr(). What do you think?

Is it possible to provide two ranges to the diagnostic so that it underlines both %s and str()?

---

_Comment by @ntBre on 2025-08-21 12:48_

> It now only flags %s with str() and %r with repr(). What do you think?

That looks great!

> Is it possible to provide two ranges to the diagnostic so that it underlines both %s and str()?

Yes, as of ~last week! The easiest way is  with `secondary_annotation` on the guard returned by `report_diagnostic`:

https://github.com/astral-sh/ruff/blob/2245355931f66def488809a6b9b784f7c294bd49/crates/ruff_linter/src/checkers/ast/mod.rs#L3309-L3311

Here's the one use of it we've added so far:

https://github.com/astral-sh/ruff/blob/2245355931f66def488809a6b9b784f7c294bd49/crates/ruff_linter/src/rules/pyflakes/rules/redefined_while_unused.rs#L195-L198

And an example of the type of diagnostic it produces (the `-----` underlining is used for secondary annotations):

https://github.com/astral-sh/ruff/blob/2245355931f66def488809a6b9b784f7c294bd49/crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__F811_F811_0.py.snap#L4-L17

There are some other options if that doesn't look good, but that's the easiest place to start.

---

_Comment by @GDYendell on 2025-08-26 20:21_

Hmm. I am not sure this is possible because `Expr::StringLiteral` only has a `range` for the entire string. Unless we wanted the secondary diagnostic to underline the whole string, which I am not sure adds much:

```
G005 Unnecessary cast `str()` in logging values when formatting with `%s`
 --> G005.py:4:14
  |
3 | # %s + str()
4 | logging.info("Hello %s", str("World!"))
  |              ----------  ^^^^^^^^^^^^^
  |              |
  |              Logging message here
5 | logging.log(logging.INFO, "Hello %s", str("World!"))
```

Thoughts?

---

_Comment by @ntBre on 2025-08-29 15:37_

Yeah, I don't think it makes sense to underline the whole string. I guess I was assuming that `CFormatString` would have a range for each part, but it doesn't look like it does. No worries, and thanks for looking into it! A single diagnostic range seems fine.

Would you mind resolving the conflicts? Hopefully they're not too bad. I'll try to give this another review soon :)

---

_Comment by @GDYendell on 2025-08-29 19:40_

Squashed and rebased to resolve conflicts.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/violations.rs`:393 on 2025-09-09 17:10_

Sorry for the hassle, but let's go ahead and make this a Ruff rule. It still feels a bit different to me from the other `G` rules, and it's not implemented upstream anyway.

I also still like the name `LoggingEagerConversion`, but the alternatives you suggested (`LoggingEagerFormat` or `LoggingEagerCast`) also sound good if you strongly prefer one of those. I'd probably rank `Cast` slightly above `Format` though :)

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/snapshots/ruff_linter__rules__flake8_logging_format__tests__G005.py.snap`:74 on 2025-09-09 17:13_

I think it might sound a little better as:


```suggestion
G005 Unnecessary `repr()` cast in logging values when formatting with `%r`
```

and if we opt not to include `cast` in the lint name, it might make more sense to say `conversion` or whatever ends up in the lint name, just to be consistent.

I think we could possibly drop the `in logging values` part of the message too, but I don't feel too strongly either way. It might still be helpful in the `concise` output format, for example.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/violations.rs`:344 on 2025-09-09 17:15_

Similar to a comment below, it might be nice to make this consistent with the name of the lint. Something like


```suggestion
/// Checks for eager string conversion of arguments to `logging` calls.
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:273 on 2025-09-09 17:21_

nit: apologies if I suggested this in a previous round of review, but this looks a bit confusing to me now. I think it would be nicer as a sequential check:



```suggestion
        let Some(Expr::StringLiteral(string_literal)) = call.arguments.find_argument_value("msg", msg_pos) else {
            return;
        }
        let Ok(format_string) = CFormatString::from_str(string_literal.value.to_str()) else {
            return;
        };
```

---

_@ntBre reviewed on 2025-09-09 17:24_

Thank you! This looks great to me. I just had a few very small suggestions, and then I think we should move this to a Ruff rule. But this is otherwise good to go.

---

_@GDYendell reviewed on 2025-09-10 23:31_

---

_Review comment by @GDYendell on `crates/ruff_linter/src/rules/flake8_logging_format/violations.rs`:393 on 2025-09-10 23:31_

No problem!

I have had a go at this. I am not sure if it is OK to import from `flake8_logging_format` in `ruff` or if these functions should go somewhere common.

Is 103 the correct next number to use? It wasn't entirely clear because of the gaps.

---

_@ntBre reviewed on 2025-09-11 13:30_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/violations.rs`:393 on 2025-09-11 13:30_

I'll take a closer look when I review again, but in principle it's fine to import helpers from `flake8_logging_format`. One day we might split up the crates more to help with compile times, but that's pretty much the only time it could be a problem, I think. And we don't have any immediate plans for that.

For the rule code, I think we can actually slot in at `RUF065`. There are two closed PRs referencing that number, so I think it should be safe to use. I meant to include that in my first message. Sorry for the trouble recoding it twice.

---

_Renamed from "Add rule G005 for redundant string pre-formatting in logging calls" to "Add rule RUF065 for uneccesary string conversion in logging calls" by @GDYendell on 2025-09-11 19:36_

---

_Renamed from "Add rule RUF065 for uneccesary string conversion in logging calls" to "Add rule RUF065 for unnecessary string conversion in logging calls" by @GDYendell on 2025-09-11 19:40_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/logging_eager_conversion.rs`:87 on 2025-09-18 15:50_

nit: this is usually a doc comment on the function itself. At least that's how I search for rules: `grep '/// RULE'` 😆 

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/logging_eager_conversion.rs`:88 on 2025-09-18 15:51_

I think we should be able to remove this check. We check this before calling `logging_eager_conversion` in `expression.rs`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/logging_eager_conversion.rs`:105 on 2025-09-18 15:52_

I hadn't noticed this before, but is this just `msg_pos + 1`?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/logging_eager_conversion.rs`:12 on 2025-09-18 15:58_

I wasn't too worried about this, but since I found a few other nits, would you mind sorting these into `std`, "third-party," and first-party blocks:


```suggestion
use std::str::FromStr;

use ruff_macros::{ViolationMetadata, derive_message_formats};
use ruff_python_ast::{self as ast, Expr};
use ruff_python_literal::cformat::{CFormatPart, CFormatString, CFormatType};
use ruff_python_literal::format::FormatConversion;
use ruff_text_size::Ranged;

use crate::Violation;
use crate::checkers::ast::Checker;
use crate::registry::Rule;
use crate::rules::flake8_logging_format::rules::{LoggingCallType, find_logging_call};
```

Unfortunately this still isn't supported by stable rustfmt, so I try to do it manually if I notice it.

---

_@ntBre approved on 2025-09-18 15:59_

Excellent, thank you! I managed to find a few more tiny nits, but this looks great.

---

_Review comment by @GDYendell on `crates/ruff_linter/src/rules/ruff/rules/logging_eager_conversion.rs`:105 on 2025-09-19 07:47_

Huh, yeah you're right. I find this a lot clearer, though. Could I suggest we change the
`msg_pos` assignment to use this match statement and then use `msg_pos + 1` for the `.skip()`?

---

_@GDYendell reviewed on 2025-09-19 07:47_

---

_@ntBre reviewed on 2025-09-19 13:34_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/logging_eager_conversion.rs`:105 on 2025-09-19 13:34_

Sure, I think that's a good idea. I always have to stop and think about the `usize::from(bool)` trick.

---

_Renamed from "Add rule RUF065 for unnecessary string conversion in logging calls" to "[`ruff`] Add `logging-eager-conversion` (`RUF065`)" by @ntBre on 2025-09-19 20:35_

---

_@ntBre approved on 2025-09-19 20:36_

Thanks again!

---

_Merged by @ntBre on 2025-09-19 20:43_

---

_Closed by @ntBre on 2025-09-19 20:43_

---
