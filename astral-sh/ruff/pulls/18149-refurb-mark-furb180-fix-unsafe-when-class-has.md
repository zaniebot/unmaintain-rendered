```yaml
number: 18149
title: "[`refurb`] Mark `FURB180` fix unsafe when class has bases"
type: pull_request
state: merged
author: robsdedude
labels:
  - bug
  - fixes
  - preview
assignees: []
merged: true
base: main
head: fix/13307-unsafe-fix
created_at: 2025-05-17T11:32:33Z
updated_at: 2025-06-03T00:54:12Z
url: https://github.com/astral-sh/ruff/pull/18149
synced_at: 2026-01-10T18:45:04Z
```

# [`refurb`] Mark `FURB180` fix unsafe when class has bases

---

_Pull request opened by @robsdedude on 2025-05-17 11:32_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Mark `FURB180`'s fix as unsafe if the class already has base classes. This is because the base classes might validate the other base classes (like `typing.Protocol` does) or otherwise alter runtime behavior if more base classes are added.

## Test Plan

The existing snapshot test covers this case already.

## References

Partially addresses https://github.com/astral-sh/ruff/issues/13307 (left out way to permit certain exceptions)


---

_Comment by @MichaReiser on 2025-05-25 10:59_

@ntBre could you take a look at this PR when you're back?

---

_Label `bug` added by @ntBre on 2025-05-29 19:22_

---

_Label `fixes` added by @ntBre on 2025-05-29 19:22_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:77 on 2025-05-29 19:29_

nit: I think we could use something like this here:

```suggestion
    let applicability = if class_def.bases().is_empty() { Applicability::Safe } else { Applicability::Unsafe }; 
```

And then instead of the `make_fix` method, we can just use `Fix::applicable_edits` where we previously used `Fix::safe_edits`. You could then move your comments from `make_fix` here too, or possibly delete them too since I think you covered it nicely in the `Fix safety` section.

---

_@ntBre approved on 2025-05-29 19:30_

Thanks, this makes sense to me! I just had one stylistic suggestion.

---

_Comment by @github-actions[bot] on 2025-05-29 19:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -24 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -6 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/2624df691a08463271017430f30fd4d17492b1ff/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L78'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:78:49:</a> FURB180 Use of `metaclass=abc.ABCMeta` to define abstract base class
- <a href='https://github.com/apache/airflow/blob/2624df691a08463271017430f30fd4d17492b1ff/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L78'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:78:49:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/apache/airflow/blob/2624df691a08463271017430f30fd4d17492b1ff/providers/standard/src/airflow/providers/standard/operators/python.py#L376'>providers/standard/src/airflow/providers/standard/operators/python.py:376:53:</a> FURB180 Use of `metaclass=abc.ABCMeta` to define abstract base class
- <a href='https://github.com/apache/airflow/blob/2624df691a08463271017430f30fd4d17492b1ff/providers/standard/src/airflow/providers/standard/operators/python.py#L376'>providers/standard/src/airflow/providers/standard/operators/python.py:376:53:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/apache/airflow/blob/2624df691a08463271017430f30fd4d17492b1ff/task-sdk/src/airflow/sdk/definitions/_internal/node.py#L67'>task-sdk/src/airflow/sdk/definitions/_internal/node.py:67:32:</a> FURB180 Use of `metaclass=abc.ABCMeta` to define abstract base class
- <a href='https://github.com/apache/airflow/blob/2624df691a08463271017430f30fd4d17492b1ff/task-sdk/src/airflow/sdk/definitions/_internal/node.py#L67'>task-sdk/src/airflow/sdk/definitions/_internal/node.py:67:32:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/fc13a0fde5b775f6734a0280742f44ddc2acdadb/superset/db_engine_specs/presto.py#L159'>superset/db_engine_specs/presto.py:159:44:</a> FURB180 Use of `metaclass=abc.ABCMeta` to define abstract base class
- <a href='https://github.com/apache/superset/blob/fc13a0fde5b775f6734a0280742f44ddc2acdadb/superset/db_engine_specs/presto.py#L159'>superset/db_engine_specs/presto.py:159:44:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L54'>src/bokeh/colors/color.py:54:27:</a> FURB180 Use of `metaclass=abc.ABCMeta` to define abstract base class
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L54'>src/bokeh/colors/color.py:54:27:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+0 -0 violations, +0 -14 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/6a19f0c462ddd63a9d91a3dbc837cd49c54253bc/astropy/cosmology/_src/tests/test_core.py#L111'>astropy/cosmology/_src/tests/test_core.py:111:5:</a> FURB180 Use of `metaclass=abc.ABCMeta` to define abstract base class
- <a href='https://github.com/astropy/astropy/blob/6a19f0c462ddd63a9d91a3dbc837cd49c54253bc/astropy/cosmology/_src/tests/test_core.py#L111'>astropy/cosmology/_src/tests/test_core.py:111:5:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/astropy/astropy/blob/6a19f0c462ddd63a9d91a3dbc837cd49c54253bc/astropy/utils/shapes.py#L167'>astropy/utils/shapes.py:167:46:</a> FURB180 Use of `metaclass=abc.ABCMeta` to define abstract base class
- <a href='https://github.com/astropy/astropy/blob/6a19f0c462ddd63a9d91a3dbc837cd49c54253bc/astropy/utils/shapes.py#L167'>astropy/utils/shapes.py:167:46:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/astropy/astropy/blob/6a19f0c462ddd63a9d91a3dbc837cd49c54253bc/astropy/visualization/wcsaxes/frame.py#L157'>astropy/visualization/wcsaxes/frame.py:157:30:</a> FURB180 Use of `metaclass=abc.ABCMeta` to define abstract base class
- <a href='https://github.com/astropy/astropy/blob/6a19f0c462ddd63a9d91a3dbc837cd49c54253bc/astropy/visualization/wcsaxes/frame.py#L157'>astropy/visualization/wcsaxes/frame.py:157:30:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/astropy/astropy/blob/6a19f0c462ddd63a9d91a3dbc837cd49c54253bc/astropy/visualization/wcsaxes/transforms.py#L149'>astropy/visualization/wcsaxes/transforms.py:149:45:</a> FURB180 Use of `metaclass=abc.ABCMeta` to define abstract base class
- <a href='https://github.com/astropy/astropy/blob/6a19f0c462ddd63a9d91a3dbc837cd49c54253bc/astropy/visualization/wcsaxes/transforms.py#L149'>astropy/visualization/wcsaxes/transforms.py:149:45:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/astropy/astropy/blob/6a19f0c462ddd63a9d91a3dbc837cd49c54253bc/astropy/visualization/wcsaxes/transforms.py#L180'>astropy/visualization/wcsaxes/transforms.py:180:45:</a> FURB180 Use of `metaclass=abc.ABCMeta` to define abstract base class
- <a href='https://github.com/astropy/astropy/blob/6a19f0c462ddd63a9d91a3dbc837cd49c54253bc/astropy/visualization/wcsaxes/transforms.py#L180'>astropy/visualization/wcsaxes/transforms.py:180:45:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/astropy/astropy/blob/6a19f0c462ddd63a9d91a3dbc837cd49c54253bc/astropy/visualization/wcsaxes/transforms.py#L31'>astropy/visualization/wcsaxes/transforms.py:31:34:</a> FURB180 Use of `metaclass=abc.ABCMeta` to define abstract base class
- <a href='https://github.com/astropy/astropy/blob/6a19f0c462ddd63a9d91a3dbc837cd49c54253bc/astropy/visualization/wcsaxes/transforms.py#L31'>astropy/visualization/wcsaxes/transforms.py:31:34:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
+ <a href='https://github.com/astropy/astropy/blob/6a19f0c462ddd63a9d91a3dbc837cd49c54253bc/astropy/wcs/wcsapi/wrappers/base.py#L6'>astropy/wcs/wcsapi/wrappers/base.py:6:39:</a> FURB180 Use of `metaclass=abc.ABCMeta` to define abstract base class
- <a href='https://github.com/astropy/astropy/blob/6a19f0c462ddd63a9d91a3dbc837cd49c54253bc/astropy/wcs/wcsapi/wrappers/base.py#L6'>astropy/wcs/wcsapi/wrappers/base.py:6:39:</a> FURB180 [*] Use of `metaclass=abc.ABCMeta` to define abstract base class
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB180 | 24 | 0 | 0 | 0 | 24 |

</p>
</details>




---

_Label `preview` added by @ntBre on 2025-05-29 19:44_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:77 on 2025-05-30 18:56_

Thank you very much. Nit happily applied.

---

_@robsdedude reviewed on 2025-05-30 18:56_

---

_@ntBre approved on 2025-06-03 00:37_

---

_Renamed from "Mark FURB180 fix unsafe when class has bases" to "[`refurb`] Mark `FURB180` fix unsafe when class has bases" by @ntBre on 2025-06-03 00:38_

---

_Merged by @ntBre on 2025-06-03 00:51_

---

_Closed by @ntBre on 2025-06-03 00:51_

---
