```yaml
number: 12538
title: "[`pydoclint`] Add `docstring-missing-yields` amd `docstring-extraneous-yields` (`DOC402`, `DOC403`)"
type: pull_request
state: merged
author: augustelalande
labels:
  - docstring
  - preview
assignees: []
merged: true
base: main
head: doc4xx
created_at: 2024-07-27T04:54:47Z
updated_at: 2024-08-02T17:08:55Z
url: https://github.com/astral-sh/ruff/pull/12538
synced_at: 2026-01-10T21:47:02Z
```

# [`pydoclint`] Add `docstring-missing-yields` amd `docstring-extraneous-yields` (`DOC402`, `DOC403`)

---

_Pull request opened by @augustelalande on 2024-07-27 04:54_

## Summary

Add `docstring-missing-yields` amd `docstring-extraneous-yields` (`DOC402`, `DOC403`). These rules check that the yields defined (or not) in the docstring of a function match its implementation.

The implementation is essentially a copy-paste of #12485

Part of #12434.

## Test Plan

Test cases added.

---

_Comment by @augustelalande on 2024-07-27 05:12_

What happened to the ecosystem check? Why isn't it making a comment of the changes?

---

_Comment by @charliermarsh on 2024-07-27 12:44_

Not sure yet, it looks like the comment job can't find the artifact: https://github.com/astral-sh/ruff/actions/runs/10121063472/job/27991584964

---

_Comment by @github-actions[bot] on 2024-07-27 16:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+205 -0 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+168 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/api/common/mark_tasks.py#L249'>airflow/api/common/mark_tasks.py:249:17:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/api/common/mark_tasks.py#L288'>airflow/api/common/mark_tasks.py:288:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/cli/cli_parser.py#L162'>airflow/cli/cli_parser.py:162:5:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/dag_processing/manager.py#L842'>airflow/dag_processing/manager.py:842:21:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/datasets/__init__.py#L224'>airflow/datasets/__init__.py:224:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/datasets/__init__.py#L304'>airflow/datasets/__init__.py:304:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/datasets/__init__.py#L344'>airflow/datasets/__init__.py:344:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/datasets/__init__.py#L403'>airflow/datasets/__init__.py:403:17:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/jobs/backfill_job_runner.py#L341'>airflow/jobs/backfill_job_runner.py:341:21:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/metrics/otel_logger.py#L374'>airflow/metrics/otel_logger.py:374:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/models/abstractoperator.py#L289'>airflow/models/abstractoperator.py:289:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/models/abstractoperator.py#L314'>airflow/models/abstractoperator.py:314:17:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/models/abstractoperator.py#L328'>airflow/models/abstractoperator.py:328:17:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/models/abstractoperator.py#L357'>airflow/models/abstractoperator.py:357:17:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/models/abstractoperator.py#L370'>airflow/models/abstractoperator.py:370:17:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/models/abstractoperator.py#L397'>airflow/models/abstractoperator.py:397:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/models/dag.py#L1241'>airflow/models/dag.py:1241:17:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/models/dag.py#L3648'>airflow/models/dag.py:3648:21:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/models/dagrun.py#L1405'>airflow/models/dagrun.py:1405:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/models/dagrun.py#L1502'>airflow/models/dagrun.py:1502:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/models/dagrun.py#L981'>airflow/models/dagrun.py:981:21:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/models/mappedoperator.py#L862'>airflow/models/mappedoperator.py:862:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/models/taskinstance.py#L2608'>airflow/models/taskinstance.py:2608:21:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/models/taskinstance.py#L383'>airflow/models/taskinstance.py:383:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/models/xcom_arg.py#L113'>airflow/models/xcom_arg.py:113:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/operators/branch.py#L65'>airflow/operators/branch.py:65:17:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/providers/airbyte/triggers/airbyte.py#L75'>airflow/providers/airbyte/triggers/airbyte.py:75:21:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/providers/amazon/aws/hooks/eks.py#L609'>airflow/providers/amazon/aws/hooks/eks.py:609:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/providers/amazon/aws/hooks/logs.py#L142'>airflow/providers/amazon/aws/hooks/logs.py:142:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/providers/amazon/aws/hooks/logs.py#L236'>airflow/providers/amazon/aws/hooks/logs.py:236:21:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/providers/amazon/aws/hooks/s3.py#L506'>airflow/providers/amazon/aws/hooks/s3.py:506:21:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/providers/amazon/aws/hooks/sagemaker.py#L1465'>airflow/providers/amazon/aws/hooks/sagemaker.py:1465:17:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/providers/amazon/aws/hooks/sagemaker.py#L276'>airflow/providers/amazon/aws/hooks/sagemaker.py:276:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/providers/amazon/aws/transfers/sql_to_s3.py#L221'>airflow/providers/amazon/aws/transfers/sql_to_s3.py:221:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/providers/amazon/aws/triggers/batch.py#L175'>airflow/providers/amazon/aws/triggers/batch.py:175:25:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/providers/amazon/aws/triggers/redshift_cluster.py#L313'>airflow/providers/amazon/aws/triggers/redshift_cluster.py:313:21:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/providers/amazon/aws/triggers/s3.py#L100'>airflow/providers/amazon/aws/triggers/s3.py:100:29:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/providers/amazon/aws/triggers/s3.py#L207'>airflow/providers/amazon/aws/triggers/s3.py:207:25:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/providers/amazon/aws/triggers/sagemaker.py#L277'>airflow/providers/amazon/aws/triggers/sagemaker.py:277:21:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/8facce407ff8aabc2ed46d52d005712328461f4e/airflow/providers/apache/beam/triggers/beam.py#L117'>airflow/providers/apache/beam/triggers/beam.py:117:13:</a> DOC402 `yield` is not documented in docstring
... 128 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+26 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/40520c54d40f887453827ef36a9f5924119ada62/scripts/cancel_github_workflows.py#L81'>scripts/cancel_github_workflows.py:81:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/40520c54d40f887453827ef36a9f5924119ada62/superset/commands/database/uploaders/columnar_reader.py#L100'>superset/commands/database/uploaders/columnar_reader.py:100:29:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/40520c54d40f887453827ef36a9f5924119ada62/superset/distributed_lock/__init__.py#L75'>superset/distributed_lock/__init__.py:75:5:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/40520c54d40f887453827ef36a9f5924119ada62/superset/extensions/metadb.py#L415'>superset/extensions/metadb.py:415:17:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/40520c54d40f887453827ef36a9f5924119ada62/superset/migrations/shared/utils.py#L156'>superset/migrations/shared/utils.py:156:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/40520c54d40f887453827ef36a9f5924119ada62/superset/models/core.py#L445'>superset/models/core.py:445:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/40520c54d40f887453827ef36a9f5924119ada62/superset/sql_parse.py#L1557'>superset/sql_parse.py:1557:17:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/40520c54d40f887453827ef36a9f5924119ada62/superset/utils/core.py#L1410'>superset/utils/core.py:1410:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/40520c54d40f887453827ef36a9f5924119ada62/superset/utils/decorators.py#L150'>superset/utils/decorators.py:150:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/40520c54d40f887453827ef36a9f5924119ada62/superset/utils/log.py#L270'>superset/utils/log.py:270:9:</a> DOC402 `yield` is not documented in docstring
... 16 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L60'>examples/basic/data/server_sent_events_source.py:60:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L72'>src/bokeh/core/query.py:72:1:</a> DOC403 Docstring has a "Yields" section but the function doesn't yield anything
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/layouts.py#L672'>src/bokeh/layouts.py:672:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/dataclasses.py#L73'>src/bokeh/util/dataclasses.py:73:17:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/selenium.py#L114'>tests/support/plugins/selenium.py:114:5:</a> DOC402 `yield` is not documented in docstring
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/69194489177bc3b43ff7002bd4de92ceda628f77/zerver/data_import/slack.py#L850'>zerver/data_import/slack.py:850:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/zulip/zulip/blob/69194489177bc3b43ff7002bd4de92ceda628f77/zerver/lib/context_managers.py#L45'>zerver/lib/context_managers.py:45:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/zulip/zulip/blob/69194489177bc3b43ff7002bd4de92ceda628f77/zerver/lib/test_helpers.py#L224'>zerver/lib/test_helpers.py:224:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/zulip/zulip/blob/69194489177bc3b43ff7002bd4de92ceda628f77/zerver/lib/test_helpers.py#L234'>zerver/lib/test_helpers.py:234:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/zulip/zulip/blob/69194489177bc3b43ff7002bd4de92ceda628f77/zerver/lib/user_groups.py#L183'>zerver/lib/user_groups.py:183:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/zulip/zulip/blob/69194489177bc3b43ff7002bd4de92ceda628f77/zerver/tests/test_events.py#L358'>zerver/tests/test_events.py:358:13:</a> DOC402 `yield` is not documented in docstring
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DOC402 | 204 | 204 | 0 | 0 | 0 |
| DOC403 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Label `docstring` added by @charliermarsh on 2024-07-29 01:39_

---

_Label `preview` added by @charliermarsh on 2024-07-29 01:39_

---

_@MichaReiser approved on 2024-08-01 21:32_

LGTM. Thank you and sorry for the wait. @AlexWaygood can you think of any other cases where a function would yield other than `yield` and `yield from`. If not, let's mege :)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:391 on 2024-08-02 11:31_

If we derived `Default` for `DocstringSections`, then this method could be:

```rs
impl<'a> DocstringSections<'a> {
    fn from_sections(sections: &'a SectionContexts, style: SectionStyle) -> Self {
        let mut sections = Self::default();
        for section in sections {
            match section.kind() {
                SectionKind::Raises => sections.raises = Some(RaisesSection::from_section(&section, style)),
                SectionKind::Returns => sections.returns = Some(GenericSection::from_section(&section)),
                SectionKind::Yields => sections.yields = Some(GenericSection::from_section(&section)),
                _ => continue,
            }
        }
        sections
    }
```
Which would be a small simplification

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:567 on 2024-08-02 11:32_

Why is `Lambda` being specifically special-cased here?

---

_@AlexWaygood reviewed on 2024-08-02 11:32_

Overall this looks great to me -- thanks @augustelalande!

---

_Comment by @AlexWaygood on 2024-08-02 11:41_

@augustelalande -- do you know exactly what the convention is with functions decorated with `@contextlib.contextmanager`? These functions contain `yield` statements but the decorator transforms the function so that it's no longer a generator function.

I can't immediately find anything about contextmanagers in the Sphinx or numpydoc docs, so I _assume_ they're treated just like other functions with `yield`s in them -- which makes sense, I think. But just checking you're not aware of any specific conventions regarding contextmanagers?

---

_@MichaReiser reviewed on 2024-08-02 12:13_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:567 on 2024-08-02 12:13_

I think the reason is because we don't want to/need to traverse into lambda bodies. Or does yielding in a lambda yield a value in the enclosing function?

---

_@AlexWaygood reviewed on 2024-08-02 12:16_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:567 on 2024-08-02 12:16_

Aha, it seems that `yield`s are invalid syntax inside `lambda`s! So it makes sense that we don't need to traverse inside them. Maybe we should add a comment?

```pycon
>>> x = lambda: yield 42
  File "<stdin>", line 1
    x = lambda: yield 42
                ^^^^^
SyntaxError: invalid syntax
>>> x = lambda: yield from ()
  File "<stdin>", line 1
    x = lambda: yield from ()
                ^^^^^
SyntaxError: invalid syntax
>>> x = lambda: yield
  File "<stdin>", line 1
    x = lambda: yield
                ^^^^^
SyntaxError: invalid syntax
```

---

_@MichaReiser reviewed on 2024-08-02 12:18_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:567 on 2024-08-02 12:18_

It matches the handling of functions and classes above. 

---

_@AlexWaygood reviewed on 2024-08-02 12:20_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:567 on 2024-08-02 12:20_

Good point. A comment isn't necessary, then.

---

_Comment by @augustelalande on 2024-08-02 16:36_

@AlexWaygood lol nice edge case. I'm not really sure, I couldn't find anything either. Trying to debate in my head, I'm pretty 50/50, both having and not having the violation make sense.

---

_@AlexWaygood approved on 2024-08-02 16:55_

Let's ship it. Thanks again!

---

_Merged by @AlexWaygood on 2024-08-02 16:55_

---

_Closed by @AlexWaygood on 2024-08-02 16:55_

---

_Branch deleted on 2024-08-02 16:56_

---
