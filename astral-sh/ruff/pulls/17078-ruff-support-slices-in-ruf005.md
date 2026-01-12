```yaml
number: 17078
title: "[`ruff`] Support slices in `RUF005`"
type: pull_request
state: merged
author: akx
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: ruf005-slice
created_at: 2025-03-31T06:09:05Z
updated_at: 2025-03-31T13:09:39Z
url: https://github.com/astral-sh/ruff/pull/17078
synced_at: 2026-01-12T15:56:00Z
```

# [`ruff`] Support slices in `RUF005`

---

_@akx_

## Summary

Teaches `RUF005` to also consider slices for concatenation. Other indexing (`foo[0] + [7, 8, 9] + bar[1]`) is explicitly not considered.

```diff
 foo = [4, 5, 6]
-bar = [1, 2, 3] + foo
-slicing1 = foo[:1] + [7, 8, 9]
-slicing2 = [7, 8, 9] + bar[1:]
-slicing3 = foo[:1] + [7, 8, 9] + bar[1:]
+bar = [1, 2, 3, *foo]
+slicing1 = [*foo[:1], 7, 8, 9]
+slicing2 = [7, 8, 9, *bar[1:]]
+slicing3 = [*foo[:1], 7, 8, 9, *bar[1:]]
```

## Test Plan

Manually tested (diff above from `ruff check --diff`), snapshot updated.


---

_Comment by @github-actions[bot] on 2025-03-31 06:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+22 -0 violations, +0 -0 fixes in 8 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/3dbed17b0ef52b3a46483219feb4b311251990eb/airflow-core/src/airflow/io/path.py#L108'>airflow-core/src/airflow/io/path.py:108:20:</a> RUF005 Consider `(parsed_url.geturl(), *args[1:])` instead of concatenation
+ <a href='https://github.com/apache/airflow/blob/3dbed17b0ef52b3a46483219feb4b311251990eb/providers/apache/spark/tests/unit/apache/spark/hooks/test_spark_jdbc_script.py#L113'>providers/apache/spark/tests/unit/apache/spark/hooks/test_spark_jdbc_script.py:113:38:</a> RUF005 Consider iterable unpacking instead of concatenation
+ <a href='https://github.com/apache/airflow/blob/3dbed17b0ef52b3a46483219feb4b311251990eb/providers/apache/spark/tests/unit/apache/spark/hooks/test_spark_jdbc_script.py#L138'>providers/apache/spark/tests/unit/apache/spark/hooks/test_spark_jdbc_script.py:138:38:</a> RUF005 Consider iterable unpacking instead of concatenation
+ <a href='https://github.com/apache/airflow/blob/3dbed17b0ef52b3a46483219feb4b311251990eb/providers/celery/src/airflow/providers/celery/executors/celery_executor.py#L263'>providers/celery/src/airflow/providers/celery/executors/celery_executor.py:263:32:</a> RUF005 Consider `(*task_tuple[:3], execute_command)` instead of concatenation
+ <a href='https://github.com/apache/airflow/blob/3dbed17b0ef52b3a46483219feb4b311251990eb/providers/google/tests/unit/google/cloud/hooks/test_cloud_storage_transfer_service.py#L859'>providers/google/tests/unit/google/cloud/hooks/test_cloud_storage_transfer_service.py:859:81:</a> RUF005 Consider `[*pages_requests[1:], None]` instead of concatenation
+ <a href='https://github.com/apache/airflow/blob/3dbed17b0ef52b3a46483219feb4b311251990eb/providers/google/tests/unit/google/cloud/hooks/test_mlengine.py#L178'>providers/google/tests/unit/google/cloud/hooks/test_mlengine.py:178:81:</a> RUF005 Consider `[*pages_requests[1:], None]` instead of concatenation
+ <a href='https://github.com/apache/airflow/blob/3dbed17b0ef52b3a46483219feb4b311251990eb/providers/google/tests/unit/google/cloud/hooks/test_mlengine.py#L807'>providers/google/tests/unit/google/cloud/hooks/test_mlengine.py:807:81:</a> RUF005 Consider `[*pages_requests[1:], None]` instead of concatenation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/b92909d621edc9a2298f4f5ea54bb07829c9ec7b/superset/viz.py#L2239'>superset/viz.py:2239:33:</a> RUF005 Consider `[DTTM_ALIAS, *groups[:i]]` instead of concatenation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/binary-husky/gpt_academic/blob/63c61e62042a44484ce53d183b22ea9382e1d086/toolbox.py#L538'>toolbox.py:538:23:</a> RUF005 Consider iterable unpacking instead of concatenation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/pie/donut.py#L41'>examples/topics/pie/donut.py:41:14:</a> RUF005 Consider `[0, *angles[:-1]]` instead of concatenation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L449'>src/bokeh/server/tornado.py:449:36:</a> RUF005 Consider `(self._prefix + p[0], *p[1:], data)` instead of concatenation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L452'>src/bokeh/server/tornado.py:452:32:</a> RUF005 Consider `(self._prefix + p[0], *p[1:])` instead of concatenation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/d26a8899625c2714a28a67e26f607c03839b8e40/tests/pyright_test.py#L40'>tests/pyright_test.py:40:15:</a> RUF005 Consider `[npx, f"pyright@{pyright_version}", *sys.argv[1:]]` instead of concatenation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/inspection/lazy_wheel.py#L115'>src/poetry/inspection/lazy_wheel.py:115:25:</a> RUF005 Consider `[start, *lslice[:1]]` instead of concatenation
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/inspection/lazy_wheel.py#L116'>src/poetry/inspection/lazy_wheel.py:116:19:</a> RUF005 Consider `[end, *rslice[-1:]]` instead of concatenation
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/console/commands/test_run.py#L133'>tests/console/commands/test_run.py:133:49:</a> RUF005 Consider `[file, *args[1:]]` instead of concatenation
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/console/commands/test_run.py#L165'>tests/console/commands/test_run.py:165:49:</a> RUF005 Consider `[file, *args[1:]]` instead of concatenation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/3d62c71146198b0a48a555a7744f12882b6cedc8/indico/modules/events/registration/fields/choices_test.py#L149'>indico/modules/events/registration/fields/choices_test.py:149:16:</a> RUF005 [*] Consider iterable unpacking instead of concatenation
+ <a href='https://github.com/indico/indico/blob/3d62c71146198b0a48a555a7744f12882b6cedc8/indico/modules/events/registration/fields/choices_test.py#L173'>indico/modules/events/registration/fields/choices_test.py:173:16:</a> RUF005 [*] Consider `[old_choices[-1], *new_choices[:-1]]` instead of concatenation
+ <a href='https://github.com/indico/indico/blob/3d62c71146198b0a48a555a7744f12882b6cedc8/indico/util/iterables.py#L68'>indico/util/iterables.py:68:18:</a> RUF005 [*] Consider `(*result[1:], elem)` instead of concatenation
+ <a href='https://github.com/indico/indico/blob/3d62c71146198b0a48a555a7744f12882b6cedc8/indico/web/forms/util.py#L60'>indico/web/forms/util.py:60:24:</a> RUF005 [*] Consider iterable unpacking instead of concatenation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/wntrblm/nox">wntrblm/nox</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/wntrblm/nox/blob/1bcb213f3630bfdeff7c7161983540a97e60de29/nox/_resolver.py#L201'>nox/_resolver.py:201:21:</a> RUF005 Consider `[*walk_list[walk_list.index(node):], node]` instead of concatenation
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF005 | 22 | 22 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @MichaReiser on 2025-03-31 08:57_

Thank you. I think we should gate this change behind preview as it significantly expands the rule's scope (as demonstrated by the ecosystem results). 

Did you review the results from the ecosystem check?

@ntBre do you want to review this change?

---

_Label `rule` added by @MichaReiser on 2025-03-31 08:57_

---

_Comment by @akx on 2025-03-31 11:23_

@MichaReiser Sure, added a second commit to gate it as `preview` only for now. (If this is merged, the two commits should probably be squashed since the second commit reverts the snapshot + test case changes from the first one.)

To my eye, the ecosystem changes look alright.

---

_Comment by @ntBre on 2025-03-31 12:05_

@MichaReiser will do!

@akx we squash and merge through GitHub, so no worries about squashing manually!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/mod.rs`:113 on 2025-03-31 12:08_

Could this be grouped with the other preview tests? It's nice when stabilizing rules just to move a `test_case` from the `preview` test to the stable test.

https://github.com/astral-sh/ruff/blob/2d7f118f52d9f7a9f8f0f5b3f6592edffc0d64b5/crates/ruff_linter/src/rules/ruff/mod.rs#L437-L439

---

_@ntBre approved on 2025-03-31 12:30_

Thanks! I had one request about the test placement, but this otherwise looks good to me.

---

_@akx reviewed on 2025-03-31 12:35_

---

_Review comment by @akx on `crates/ruff_linter/src/rules/ruff/mod.rs`:113 on 2025-03-31 12:35_

Done deal! I hadn't noticed there was a parametrized test for various preview rules, so just slipped this in there.

---

_Review requested from @ntBre by @akx on 2025-03-31 12:38_

---

_@ntBre approved on 2025-03-31 12:39_

---

_Label `preview` added by @ntBre on 2025-03-31 13:00_

---

_Renamed from "Support slices in RUF005" to "[`ruff`] Support slices in `RUF005`" by @ntBre on 2025-03-31 13:00_

---

_Merged by @ntBre on 2025-03-31 13:09_

---

_Closed by @ntBre on 2025-03-31 13:09_

---
