```yaml
number: 13148
title: "[`pydoclint`] Permit yielding `None` in DOC402 and DOC403"
type: pull_request
state: merged
author: tjkuson
labels:
  - bug
  - docstring
  - preview
assignees: []
merged: true
base: main
head: doc-yield-none
created_at: 2024-08-29T07:58:16Z
updated_at: 2024-09-01T01:49:18Z
url: https://github.com/astral-sh/ruff/pull/13148
synced_at: 2026-01-10T21:38:32Z
```

# [`pydoclint`] Permit yielding `None` in DOC402 and DOC403

---

_Pull request opened by @tjkuson on 2024-08-29 07:58_

## Summary

Currently, [docstring-extraneous-yields (DOC403)](https://docs.astral.sh/ruff/rules/docstring-extraneous-yields/) reports a diagnostic if there is a yields section in a docstring for a function that yields `None` implicitly; for example,

```python
from collections import abc

def gen() -> abc.Generator[None, None, None]:
    """A very helpful docstring.

    Yields:
        When foo.
    """
    ...  # some code
    yield
    ...  # some more code
```

This is problematic, as sometimes you want to document this yielding behaviour.

This patch permits such docstrings by now counting implicit `yield None` statements as yields. It then updates [docstring-missing-yields (DOC402)](https://docs.astral.sh/ruff/rules/docstring-missing-yields/) to allow users to forego a yield section if the function is resolved to yield `None` only (by checking the return annotation, or by checking the yield statements if the return type is not annotated).

This is similar to #13064

Closes #13001

## Test Plan

`cargo nextest run`


---

_Renamed from "[`pydoclint`] Permit yielding None in DOC402 and DOC403" to "[`pydoclint`] Permit yielding `None` in DOC402 and DOC403" by @tjkuson on 2024-08-29 08:05_

---

_Marked ready for review by @tjkuson on 2024-08-29 08:06_

---

_Comment by @github-actions[bot] on 2024-08-29 08:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+6 -41 violations, +0 -0 fixes in 3 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+5 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/3b76ec9a895484bd65bac7062ee81826c81d5304/airflow/cli/cli_config.py#L140'>airflow/cli/cli_config.py:140:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/3b76ec9a895484bd65bac7062ee81826c81d5304/airflow/cli/cli_config.py#L141'>airflow/cli/cli_config.py:141:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/3b76ec9a895484bd65bac7062ee81826c81d5304/airflow/metrics/otel_logger.py#L198'>airflow/metrics/otel_logger.py:198:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/3b76ec9a895484bd65bac7062ee81826c81d5304/airflow/metrics/otel_logger.py#L205'>airflow/metrics/otel_logger.py:205:13:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/3b76ec9a895484bd65bac7062ee81826c81d5304/airflow/metrics/otel_logger.py#L224'>airflow/metrics/otel_logger.py:224:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/3b76ec9a895484bd65bac7062ee81826c81d5304/airflow/metrics/otel_logger.py#L231'>airflow/metrics/otel_logger.py:231:13:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/3b76ec9a895484bd65bac7062ee81826c81d5304/airflow/providers/airbyte/sensors/airbyte.py#L127'>airflow/providers/airbyte/sensors/airbyte.py:127:17:</a> DOC201 `return` is not documented in docstring
... 1 additional changes omitted for rule DOC201
- <a href='https://github.com/apache/airflow/blob/3b76ec9a895484bd65bac7062ee81826c81d5304/airflow/providers/amazon/aws/hooks/logs.py#L142'>airflow/providers/amazon/aws/hooks/logs.py:142:13:</a> DOC402 `yield` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/3b76ec9a895484bd65bac7062ee81826c81d5304/airflow/providers/amazon/aws/hooks/sagemaker.py#L276'>airflow/providers/amazon/aws/hooks/sagemaker.py:276:13:</a> DOC402 `yield` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/3b76ec9a895484bd65bac7062ee81826c81d5304/airflow/providers/grpc/hooks/grpc.py#L137'>airflow/providers/grpc/hooks/grpc.py:137:21:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/3b76ec9a895484bd65bac7062ee81826c81d5304/airflow/triggers/base.py#L102'>airflow/triggers/base.py:102:9:</a> DOC402 `yield` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/3b76ec9a895484bd65bac7062ee81826c81d5304/tests/test_utils/asserts.py#L159'>tests/test_utils/asserts.py:159:9:</a> DOC402 `yield` is not documented in docstring
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/548d543efe81ecd6f0a6657550230b765ab4d955/superset/utils/core.py#L1305'>superset/utils/core.py:1305:13:</a> DOC402 `yield` is not documented in docstring
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -34 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- doc/source/user_guide/style.ipynb:cell 113:31:89: E501 Line too long (90 > 88)
- doc/source/user_guide/style.ipynb:cell 113:33:89: E501 Line too long (98 > 88)
- doc/source/user_guide/style.ipynb:cell 123:5:56: E741 Ambiguous variable name: `l`
- doc/source/user_guide/style.ipynb:cell 125:2:13: C408 Unnecessary `dict` call (rewrite as a literal)
- doc/source/user_guide/style.ipynb:cell 125:4:13: C408 Unnecessary `dict` call (rewrite as a literal)
- doc/source/user_guide/style.ipynb:cell 125:6:13: C408 Unnecessary `dict` call (rewrite as a literal)
- doc/source/user_guide/style.ipynb:cell 125:8:13: C408 Unnecessary `dict` call (rewrite as a literal)
- doc/source/user_guide/style.ipynb:cell 126:1:1: NPY002 Replace legacy `np.random.seed` call with `np.random.Generator`
- doc/source/user_guide/style.ipynb:cell 126:2:1: PLW0127 Self-assignment of variable `cmap`
- doc/source/user_guide/style.ipynb:cell 126:2:8: PLW0128 Redeclared variable `cmap` in assignment
- doc/source/user_guide/style.ipynb:cell 126:3:22: NPY002 Replace legacy `np.random.randn` call with `np.random.Generator`
- doc/source/user_guide/style.ipynb:cell 128:1:22: NPY002 Replace legacy `np.random.randn` call with `np.random.Generator`
- doc/source/user_guide/style.ipynb:cell 134:1:89: E501 Line too long (93 > 88)
- doc/source/user_guide/style.ipynb:cell 141:1:89: E501 Line too long (95 > 88)
- doc/source/user_guide/style.ipynb:cell 157:1:38: F811 Redefinition of unused `f` from cell 123, line 5
- doc/source/user_guide/style.ipynb:cell 15:2:89: E501 Line too long (103 > 88)
- doc/source/user_guide/style.ipynb:cell 15:3:89: E501 Line too long (156 > 88)
... 13 additional changes omitted for rule E501
- doc/source/user_guide/style.ipynb:cell 37:1:1: NPY002 Replace legacy `np.random.seed` call with `np.random.Generator`
- doc/source/user_guide/style.ipynb:cell 37:2:20: NPY002 Replace legacy `np.random.randn` call with `np.random.Generator`
- doc/source/user_guide/style.ipynb:cell 60:1:20: NPY002 Replace legacy `np.random.randn` call with `np.random.Generator`
... 3 additional changes omitted for rule NPY002
</pre>

</p>
</details>
<details><summary>Changes by rule (9 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E501 | 18 | 0 | 18 | 0 | 0 |
| NPY002 | 8 | 0 | 8 | 0 | 0 |
| DOC201 | 7 | 4 | 3 | 0 | 0 |
| DOC402 | 6 | 2 | 4 | 0 | 0 |
| C408 | 4 | 0 | 4 | 0 | 0 |
| E741 | 1 | 0 | 1 | 0 | 0 |
| PLW0127 | 1 | 0 | 1 | 0 | 0 |
| PLW0128 | 1 | 0 | 1 | 0 | 0 |
| F811 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @tjkuson on 2024-08-29 08:14_

Oh I should add `Iterator` etc to the generator types

---

_Comment by @AlexWaygood on 2024-08-29 09:29_

So, consider this ecosystem hit: <https://github.com/apache/superset/blob/07985e2f5aa165f6868abbf88594e6d75300caae/superset/utils/core.py#L1288-L1312>. The yield is "documented" there in that it's clear that the function is a context manager that only _temporarily_ overrides a value. But there's no "Yields" section -- and I think it would make the docstring worse if they rewrote it to include a "Yields" section; but this change would mean that we'd emit DOC402 on that docstring.

I think my preferred behaviour here would be:
- Don't report `DOC402` for a function that only ever has bare `yield` or `yield None` statements
- But also, don't report `DOC403` just _because_ the `yield` is documented

I think this is what you suggested in https://github.com/astral-sh/ruff/issues/13001#issuecomment-2307604996 (sorry that nobody responded to you there!). It's a little different from numpydoc's behaviour IIUC, but I think that's okay.

---

_Label `bug` added by @AlexWaygood on 2024-08-29 09:29_

---

_Label `docstring` added by @AlexWaygood on 2024-08-29 09:29_

---

_Label `preview` added by @AlexWaygood on 2024-08-29 09:29_

---

_Comment by @AlexWaygood on 2024-08-29 09:30_

(Oh and thanks for the PR! Sorry for diving straight into the review 😄)

---

_Comment by @tjkuson on 2024-08-29 19:03_

> So, consider this ecosystem hit: https://github.com/apache/superset/blob/07985e2f5aa165f6868abbf88594e6d75300caae/superset/utils/core.py#L1288-L1312. 

Would you mind clarifying this and how it interacts with the current implementation of `DOC201`? From what I understand, its annotated yield type is `typing.Any`, so its yield behaviour *should* be documented (it's not `None`).

If the recommendation is to ignore the type annotation if we determine all yields to be `None` by parsing the function body, this would be inconsistent with `DOC201` behaviour of treating the return annotation as truth https://github.com/astral-sh/ruff/pull/13064. Would that implementation have to change as well, or would it just be a documented discrepancy?

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-08-30 07:44_

---

_Comment by @AlexWaygood on 2024-08-30 13:39_

> > So, consider this ecosystem hit: [apache/superset@`07985e2`/superset/utils/core.py#L1288-L1312](https://github.com/apache/superset/blob/07985e2f5aa165f6868abbf88594e6d75300caae/superset/utils/core.py#L1288-L1312).
> 
> Would you mind clarifying this and how it interacts with the current implementation of `DOC201`? From what I understand, its annotated yield type is `typing.Any`, so its yield behaviour _should_ be documented (it's not `None`).

Ahh, thanks for pointing that out. Your argument makes sense; I agree that it's important for these rules to remain consistent.

Your PR however creates a new inconsistency between the two rules as it currently stands, however. `DOC201` will currently complain about the first of these two docstrings, but not the second:

```py
def foo() -> int | None:
    """Foo-y method"""
    return None


def bar() -> int | None:
    """Bar-y method"""
    return
```

Would you like to fix that as well as part of this PR, so that the rules stay consistent?

---

_Comment by @tjkuson on 2024-08-30 18:35_

> Would you like to fix that as well as part of this PR, so that the rules stay consistent?

Fixed in https://github.com/astral-sh/ruff/pull/13148/commits/79fad1c73c829372355d6ae8ea7f0e778f1bdfda!

---

_Comment by @codspeed-hq[bot] on 2024-08-30 18:40_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/tjkuson:doc-yield-none)

### Merging #13148 will **not alter performance**

<sub>Comparing <code>tjkuson:doc-yield-none</code> (117fdcc) with <code>main</code> (0c23b86)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @charliermarsh on 2024-08-30 22:25_

This makes sense to me, but I'll defer to @AlexWaygood to approve and merge.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pydoclint/DOC201_numpy.py`:194 on 2024-08-31 19:05_

lol, love that you copied this straight from my example 😆

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:737 on 2024-08-31 20:17_

Unfortunately while `Generator[int, str, None]` has an `Expr::Tuple(_)` inside the `Generator[]` slice, `Iterator[Any]` has an `Expr::Name(_)` inside the `Iterator` slice. So the logic you're adding currently works for functions returning `Generator[]` but not those returning `Iterator[]`. I'm working on a fix for this and a few more edge cases I spotted; I'll push them to your PR branch

---

_@AlexWaygood reviewed on 2024-08-31 20:18_

---

_@tjkuson reviewed on 2024-08-31 20:54_

---

_Review comment by @tjkuson on `crates/ruff_linter/resources/test/fixtures/pydoclint/DOC201_numpy.py`:194 on 2024-08-31 20:54_

Can't build upon perfection

---

_@tjkuson reviewed on 2024-08-31 20:55_

---

_Review comment by @tjkuson on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:737 on 2024-08-31 20:55_

Well spotted, thank you!

---

_Merged by @AlexWaygood on 2024-09-01 01:03_

---

_Closed by @AlexWaygood on 2024-09-01 01:03_

---

_Comment by @AlexWaygood on 2024-09-01 01:03_

Thanks @tjkuson!

---

_Branch deleted on 2024-09-01 01:49_

---
