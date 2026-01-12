```yaml
number: 21154
title: "[`pydocstyle`] Refactor `D417` to use the `pydoclint` docstring processing"
type: pull_request
state: open
author: augustelalande
labels: []
assignees: []
base: main
head: d417-refactor
created_at: 2025-10-30T23:52:24Z
updated_at: 2025-11-13T22:12:08Z
url: https://github.com/astral-sh/ruff/pull/21154
synced_at: 2026-01-12T15:57:17Z
```

# [`pydocstyle`] Refactor `D417` to use the `pydoclint` docstring processing

---

_@augustelalande_

## Summary

This PR refactors `D417` to use the parameters detected in the `pydoclint` scope, the goal is to have a `D417` implementation which is equivalent to the previous one, but uses the `pydoclint` docstring processing. Later on `D417` can be renamed to `DOC101` and `D417` deprecated. This will make deploying `DOC101` easier.

## Test Plan

The refactor is tested on the old fixtures, and the new snap was manually diffed. The following changes were found between the old and new implementation.

In the following the new implementation reports an error which the old didn't. Notice that the code mixes google and numpy style elements which makes the docstring invalid for both.

```python
def f(x):
    """Do something with valid description.

    Args:
    ----
        x: the value

    Returns:
    -------
        the value
    """
    return x
```

In the following, the old implementation reports an error but the new one does not. This is because the old implementation enforces argument order in the docstring which the new one does not. Pydoclint has a seperate rule for checking documentation order which can be implemented later.
```python
    @classmethod
    @expect("D417: Missing argument descriptions in the docstring "
            "(argument(s) test, y, z are missing descriptions in "
            "'test_missing_args_class_method' docstring)", arg_count=4)
    def test_missing_args_class_method(cls, test, x, y, z=3):  # noqa: D213, D407
        """Test a valid args section.

        Parameters
        ----------
        z
        x
            Another parameter. The parameters y, test below are
            missing descriptions. The parameter z above is also missing
            a description.
        y
        test

        """
```


---

_Converted to draft by @augustelalande on 2025-10-30 23:52_

---

_Renamed from "[`pydocstyle`] Refactor `D417` to use the `pydoclint` scope" to "[`pydocstyle`] Refactor `D417` to use the `pydoclint` docstring processing" by @augustelalande on 2025-10-30 23:53_

---

_Comment by @github-actions[bot] on 2025-10-31 00:02_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -7 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/008c7c6517071e4b111f6232fa2477084bcb1215/scripts/erd/erd.py#L84'>scripts/erd/erd.py:84:5:</a> D417 Missing argument descriptions in the docstring for `introspect_sqla_model`: `mapper`, `seen`
- <a href='https://github.com/apache/superset/blob/008c7c6517071e4b111f6232fa2477084bcb1215/superset/migrations/shared/catalogs.py#L131'>superset/migrations/shared/catalogs.py:131:5:</a> D417 Missing argument descriptions in the docstring for `print_processed_batch`: `batch_size`, `model`, `offset`, `start_time`, `total_rows`
- <a href='https://github.com/apache/superset/blob/008c7c6517071e4b111f6232fa2477084bcb1215/superset/migrations/shared/catalogs.py#L166'>superset/migrations/shared/catalogs.py:166:5:</a> D417 Missing argument descriptions in the docstring for `update_catalog_column`: `catalog`, `database`, `downgrade`, `session`
- <a href='https://github.com/apache/superset/blob/008c7c6517071e4b111f6232fa2477084bcb1215/superset/migrations/shared/catalogs.py#L295'>superset/migrations/shared/catalogs.py:295:5:</a> D417 Missing argument descriptions in the docstring for `delete_models_non_default_catalog`: `catalog`, `database`, `session`
- <a href='https://github.com/apache/superset/blob/008c7c6517071e4b111f6232fa2477084bcb1215/superset/utils/pandas_postprocessing/histogram.py#L24'>superset/utils/pandas_postprocessing/histogram.py:24:5:</a> D417 Missing argument descriptions in the docstring for `histogram`: `bins`, `column`, `cumulative`, `df`, `groupby`, `normalize`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L421'>src/bokeh/core/has_props.py:421:9:</a> D417 Missing argument description in the docstring for `set_from_json`: `value`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L421'>src/bokeh/core/has_props.py:421:9:</a> D417 Missing argument descriptions in the docstring for `set_from_json`: `setter`, `value`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/models.py#L257'>src/bokeh/document/models.py:257:9:</a> D417 Missing argument descriptions in the docstring for `update_name`: `new_name`, `old_name`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/model.py#L326'>src/bokeh/model/model.py:326:9:</a> D417 Missing argument descriptions in the docstring for `js_link`: `attr_selector`, `other`, `other_attr`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/resources.py#L194'>src/bokeh/resources.py:194:5:</a> DOC501 Raised exception `RuntimeError` missing from docstring
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D417 | 9 | 2 | 7 | 0 | 0 |
| DOC501 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>





---

_Marked ready for review by @augustelalande on 2025-11-01 06:20_

---

_Comment by @augustelalande on 2025-11-12 23:15_

The ecosystem changes are minimal, with the new implementation removing some old false positives. It introduces some new positives, caused by improper formatting which are debatably true positives in my opinion.

---

_Review requested from @ntBre by @ntBre on 2025-11-13 22:11_

---

_Comment by @ntBre on 2025-11-13 22:12_

Thanks for the merge and taking a look at the ecosystem results. This is in my inbox to have a look!

---
