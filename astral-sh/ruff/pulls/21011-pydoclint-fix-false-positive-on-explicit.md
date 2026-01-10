```yaml
number: 21011
title: "[`pydoclint`] Fix false positive on explicit exception re-raising (`DOC501`, `DOC502`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-20973
created_at: 2025-10-21T02:08:16Z
updated_at: 2025-10-24T20:54:10Z
url: https://github.com/astral-sh/ruff/pull/21011
synced_at: 2026-01-10T16:59:49Z
```

# [`pydoclint`] Fix false positive on explicit exception re-raising (`DOC501`, `DOC502`)

---

_Pull request opened by @danparizher on 2025-10-21 02:08_


## Summary
Fixes #20973 (`docstring-extraneous-exception`) false positive when exceptions mentioned in docstrings are caught and explicitly re-raised using `raise e` or `raise e from None`.

## Problem Analysis
The DOC502 rule was incorrectly flagging exceptions mentioned in docstrings as "not explicitly raised" when they were actually being explicitly re-raised through exception variables bound in `except` clauses. 

**Root Cause**: The `BodyVisitor` in `check_docstring.rs` only checked for direct exception references (like `raise OSError()`) but didn't recognize when a variable bound to an exception in an `except` clause was being re-raised.

**Example of the bug**:
```python
def f():
    """Do nothing.

    Raises
    ------
    OSError
        If the OS errors.
    """
    try:
        pass
    except OSError as e:
        raise e  # This was incorrectly flagged as not explicitly raising OSError
```

The issue occurred because `resolve_qualified_name(e)` couldn't resolve the variable `e` to a qualified exception name, since `e` is just a variable binding, not a direct reference to an exception class.

## Approach
Modified the `BodyVisitor` in `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs` to:

1. **Track exception variable bindings**: Added `exception_variables` field to map exception variable names to their exception types within `except` clauses
2. **Enhanced raise statement detection**: Updated `visit_stmt` to check if a `raise` statement uses a variable name that's bound to an exception in the current `except` clause
3. **Proper scope management**: Clear exception variable mappings when leaving `except` handlers to prevent cross-contamination

**Key changes**:
- Added `exception_variables: FxHashMap<&'a str, QualifiedName<'a>>` to track variable-to-exception mappings
- Enhanced `visit_except_handler` to store exception variable bindings when entering `except` clauses
- Modified `visit_stmt` to check for variable-based re-raising: `raise e` → lookup `e` in `exception_variables`
- Clear mappings when exiting `except` handlers to maintain proper scope


---

_Comment by @github-actions[bot] on 2025-10-21 02:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+92 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+91 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/client.py#L307'>airflow-ctl/src/airflowctl/api/client.py:307:5:</a> DOC501 Raised exception `AirflowCtlNotFoundException` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L190'>airflow-ctl/src/airflowctl/api/operations.py:190:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L204'>airflow-ctl/src/airflowctl/api/operations.py:204:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L212'>airflow-ctl/src/airflowctl/api/operations.py:212:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L230'>airflow-ctl/src/airflowctl/api/operations.py:230:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L241'>airflow-ctl/src/airflowctl/api/operations.py:241:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L249'>airflow-ctl/src/airflowctl/api/operations.py:249:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L259'>airflow-ctl/src/airflowctl/api/operations.py:259:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L267'>airflow-ctl/src/airflowctl/api/operations.py:267:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L275'>airflow-ctl/src/airflowctl/api/operations.py:275:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L283'>airflow-ctl/src/airflowctl/api/operations.py:283:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L291'>airflow-ctl/src/airflowctl/api/operations.py:291:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L303'>airflow-ctl/src/airflowctl/api/operations.py:303:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L311'>airflow-ctl/src/airflowctl/api/operations.py:311:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L319'>airflow-ctl/src/airflowctl/api/operations.py:319:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L332'>airflow-ctl/src/airflowctl/api/operations.py:332:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L340'>airflow-ctl/src/airflowctl/api/operations.py:340:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L348'>airflow-ctl/src/airflowctl/api/operations.py:348:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L360'>airflow-ctl/src/airflowctl/api/operations.py:360:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L368'>airflow-ctl/src/airflowctl/api/operations.py:368:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L380'>airflow-ctl/src/airflowctl/api/operations.py:380:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L395'>airflow-ctl/src/airflowctl/api/operations.py:395:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L403'>airflow-ctl/src/airflowctl/api/operations.py:403:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L411'>airflow-ctl/src/airflowctl/api/operations.py:411:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L419'>airflow-ctl/src/airflowctl/api/operations.py:419:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L430'>airflow-ctl/src/airflowctl/api/operations.py:430:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L443'>airflow-ctl/src/airflowctl/api/operations.py:443:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L455'>airflow-ctl/src/airflowctl/api/operations.py:455:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L463'>airflow-ctl/src/airflowctl/api/operations.py:463:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L529'>airflow-ctl/src/airflowctl/api/operations.py:529:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L559'>airflow-ctl/src/airflowctl/api/operations.py:559:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L586'>airflow-ctl/src/airflowctl/api/operations.py:586:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L598'>airflow-ctl/src/airflowctl/api/operations.py:598:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L606'>airflow-ctl/src/airflowctl/api/operations.py:606:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L614'>airflow-ctl/src/airflowctl/api/operations.py:614:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L622'>airflow-ctl/src/airflowctl/api/operations.py:622:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L644'>airflow-ctl/src/airflowctl/api/operations.py:644:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L656'>airflow-ctl/src/airflowctl/api/operations.py:656:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L664'>airflow-ctl/src/airflowctl/api/operations.py:664:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L672'>airflow-ctl/src/airflowctl/api/operations.py:672:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L680'>airflow-ctl/src/airflowctl/api/operations.py:680:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/airflow-ctl/src/airflowctl/api/operations.py#L694'>airflow-ctl/src/airflowctl/api/operations.py:694:9:</a> DOC501 Raised exception `ServerResponseError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/providers/amazon/src/airflow/providers/amazon/aws/hooks/cloud_formation.py#L50'>providers/amazon/src/airflow/providers/amazon/aws/hooks/cloud_formation.py:50:9:</a> DOC501 Raised exception `ClientError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/providers/amazon/src/airflow/providers/amazon/aws/hooks/dms.py#L257'>providers/amazon/src/airflow/providers/amazon/aws/hooks/dms.py:257:9:</a> DOC501 Raised exception `ClientError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/providers/amazon/src/airflow/providers/amazon/aws/hooks/dms.py#L321'>providers/amazon/src/airflow/providers/amazon/aws/hooks/dms.py:321:9:</a> DOC501 Raised exception `ClientError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/providers/amazon/src/airflow/providers/amazon/aws/hooks/dynamodb.py#L87'>providers/amazon/src/airflow/providers/amazon/aws/hooks/dynamodb.py:87:9:</a> DOC501 Raised exception `ClientError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/providers/amazon/src/airflow/providers/amazon/aws/hooks/logs.py#L143'>providers/amazon/src/airflow/providers/amazon/aws/hooks/logs.py:143:9:</a> DOC501 Raised exception `ClientError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/providers/amazon/src/airflow/providers/amazon/aws/hooks/s3.py#L1537'>providers/amazon/src/airflow/providers/amazon/aws/hooks/s3.py:1537:9:</a> DOC501 Raised exception `ClientError` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/60c38de195f698c24d1d3b18333ef1d69fc314c2/providers/amazon/src/airflow/providers/amazon/aws/hooks/s3.py#L1647'>providers/amazon/src/airflow/providers/amazon/aws/hooks/s3.py:1647:9:</a> DOC501 Raised exception `ClientError` missing from docstring
... 42 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/views/ws.py#L182'>src/bokeh/server/views/ws.py:182:9:</a> DOC501 Raised exception `ProtocolError` missing from docstring
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DOC501 | 92 | 92 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @danparizher on 2025-10-21 04:29_

The +109 DOC501 violations represent improved detection accuracy rather than regressions.

From what I'm seeing, the fix correctly identifies cases where exceptions are explicitly re-raised through variables like `raise e` but weren't being detected before.

Previously, DOC501 missed these patterns because it couldn't resolve the variable `e` to an exception type, but now it properly recognizes that `raise e` is indeed raising the exception that `e` is bound to.

---

_Label `bug` added by @ntBre on 2025-10-21 16:50_

---

_Label `rule` added by @ntBre on 2025-10-21 16:50_

---

_Label `preview` added by @ntBre on 2025-10-21 16:50_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:888 on 2025-10-21 16:55_

Why would we only store the first element in a tuple of exceptions? It seems like we should store all of them to me.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydoclint/snapshots/ruff_linter__rules__pydoclint__tests__docstring-missing-exception_DOC501_google.py.snap`:82 on 2025-10-21 17:01_

We shouldn't do this in this PR, but this looks like a nice place for  a secondary annotation showing where the `ZeroDivisionError` is raised.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pydoclint/DOC502_google.py`:109 on 2025-10-21 17:03_

I think it would be good to include an example or two of a tuple of exceptions too.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pydoclint/DOC502_google.py`:1 on 2025-10-21 17:15_

These tests don't actually trigger DOC502 on main because they're numpy-style docstrings, not Google style. We need to either rewrite them to be Google style or move them to the DOC502_numpy.py file.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:832 on 2025-10-21 17:32_

I was wondering if we could avoid collecting these here and instead try to resolve the name down below,  but I tried it locally, and it seems that we're not actually in this `Scope` in the `SemanticModel` during this check, so this makes sense to me.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:878 on 2025-10-21 17:40_

I think it might make sense to defer this as resolving qualified names can be somewhat expensive. Then we might be able to reuse the `maybe_store_exception` closure below. In other words, just store the `name.id.as_str()` and `exceptions` in the map here.

That will also help to resolve my other comment about tuples because we can just store the name -> tuple mapping and handle the tuple like the `currently_suspended_exceptions` case too.

---

_@ntBre requested changes on 2025-10-21 17:46_

Thanks! This looks like you're on the right track, but we need to make sure the tests would actually trigger on main, expand them to cover the tricky tuple case, and then I think we can simplify the code a bit too.

This is a minor detail, but would you try to update the PR title too? As the ecosystem report shows (thanks for your analysis there, it looked accurate when I spot-checked), these changes actually have a much larger impact on DOC501 than DOC502. We should mention both rules, though.

---

_Renamed from "[`pydoclint`] Fix false positive on explicit exception re-raising (`DOC502`)" to "[pydoclint] Fix false positive on explicit exception re-raising (DOC501, DOC502)" by @danparizher on 2025-10-24 03:29_

---

_Renamed from "[pydoclint] Fix false positive on explicit exception re-raising (DOC501, DOC502)" to "[pydoclint] Fix false positive on explicit exception re-raising (`DOC501`, `DOC502`)" by @danparizher on 2025-10-24 03:29_

---

_Renamed from "[pydoclint] Fix false positive on explicit exception re-raising (`DOC501`, `DOC502`)" to "[`pydoclint`] Fix false positive on explicit exception re-raising (`DOC501`, `DOC502`)" by @danparizher on 2025-10-24 03:29_

---

_Review requested from @ntBre by @danparizher on 2025-10-24 03:39_

---

_@ntBre approved on 2025-10-24 20:41_

Thanks! This looks good. I pushed one commit factoring out the closure into a method instead of duplicating it. I'll just make sure I didn't break anything in the ecosystem report and then merge this.

---

_Merged by @ntBre on 2025-10-24 20:54_

---

_Closed by @ntBre on 2025-10-24 20:54_

---
