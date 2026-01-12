```yaml
number: 12369
title: "Allow `repeated-equality-comparison` for mixed operations"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/mixed
created_at: 2024-07-17T15:36:01Z
updated_at: 2024-07-18T15:16:42Z
url: https://github.com/astral-sh/ruff/pull/12369
synced_at: 2026-01-12T15:55:41Z
```

# Allow `repeated-equality-comparison` for mixed operations

---

_@charliermarsh_

## Summary

This PR allows us to fix both expressions in `foo == "a" or foo == "b" or ("c" != bar and "d" != bar)`, but limits the rule to consecutive comparisons, following https://github.com/astral-sh/ruff/issues/7797.

I think this logic was _probably_ added because of https://github.com/astral-sh/ruff/pull/12368 -- the intent being that we'd replace the _entire_ expression.


---

_Label `rule` added by @charliermarsh on 2024-07-17 15:36_

---

_Review requested from @dhruvmanila by @charliermarsh on 2024-07-17 15:36_

---

_Comment by @charliermarsh on 2024-07-17 15:42_

Actually, I may change this to only merge adjacent values, like https://github.com/astral-sh/ruff/issues/7797.

---

_Comment by @github-actions[bot] on 2024-07-17 16:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+12 -0 violations, +0 -0 fixes in 7 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/588d4b27248470f2e6cab8174e118026ef80f59e/airflow/providers/cncf/kubernetes/triggers/pod.py#L313'>airflow/providers/cncf/kubernetes/triggers/pod.py:313:13:</a> PLR1714 Consider merging multiple comparisons. Use a `set` if the elements are hashable.
+ <a href='https://github.com/apache/airflow/blob/588d4b27248470f2e6cab8174e118026ef80f59e/airflow/providers/teradata/operators/teradata_compute_cluster.py#L122'>airflow/providers/teradata/operators/teradata_compute_cluster.py:122:13:</a> PLR1714 Consider merging multiple comparisons: `self.compute_profile_name in ("None", "")`. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/1e412a871174b1ab632ca1c95c5df5c39c20ecbf/superset/utils/core.py#L311'>superset/utils/core.py:311:13:</a> PLR1714 Consider merging multiple comparisons: `standalone_param not in ("false", "0")`. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L280'>src/bokeh/core/has_props.py:280:24:</a> PLR1714 Consider merging multiple comparisons: `head in ("bokeh", "__main__")`. Use a `set` if the elements are hashable.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_layouts.py#L292'>tests/unit/bokeh/test_layouts.py:292:12:</a> PLR1714 Consider merging multiple comparisons: `t5 not in (save0, save1)`. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/fc8fc82233e7910f5748acbc24ea6df77f4a233a/pandas/core/dtypes/astype.py#L128'>pandas/core/dtypes/astype.py:128:8:</a> PLR1714 Consider merging multiple comparisons: `object in (arr.dtype, dtype)`. Use a `set` if the elements are hashable.
+ <a href='https://github.com/pandas-dev/pandas/blob/fc8fc82233e7910f5748acbc24ea6df77f4a233a/pandas/core/nanops.py#L460'>pandas/core/nanops.py:460:13:</a> PLR1714 Consider merging multiple comparisons: `values.dtype not in (object, bool)`. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/c615022dcbb55c955bd7536dfb44758cf1d8d75c/rotkehlchen/__main__.py#L22'>rotkehlchen/__main__.py:22:12:</a> PLR1714 Consider merging multiple comparisons: `e.code in (0, 2)`. Use a `set` if the elements are hashable.
+ <a href='https://github.com/rotki/rotki/blob/c615022dcbb55c955bd7536dfb44758cf1d8d75c/rotkehlchen/chain/evm/decoding/curve/decoder.py#L542'>rotkehlchen/chain/evm/decoding/curve/decoder.py:542:18:</a> PLR1714 Consider merging multiple comparisons: `event.asset in (self.native_currency, sold_asset)`. Use a `set` if the elements are hashable.
+ <a href='https://github.com/rotki/rotki/blob/c615022dcbb55c955bd7536dfb44758cf1d8d75c/rotkehlchen/chain/evm/decoding/curve/decoder.py#L554'>rotkehlchen/chain/evm/decoding/curve/decoder.py:554:18:</a> PLR1714 Consider merging multiple comparisons. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/97753fe3b03f2fb920020e733aa43a6d108bbfd1/zerver/webhooks/opbeat/view.py#L82'>zerver/webhooks/opbeat/view.py:82:16:</a> PLR1714 Consider merging multiple comparisons: `key_raw not in ("html_url", "subject")`. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/wntrblm/nox">wntrblm/nox</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/wntrblm/nox/blob/7a77e805c6cc7a8d2b65467683da14c2b376e35a/nox/manifest.py#L322'>nox/manifest.py:322:16:</a> PLR1714 Consider merging multiple comparisons: `session in (s, s.name)`. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1714 | 12 | 12 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+12 -0 violations, +0 -0 fixes in 7 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/588d4b27248470f2e6cab8174e118026ef80f59e/airflow/providers/cncf/kubernetes/triggers/pod.py#L313'>airflow/providers/cncf/kubernetes/triggers/pod.py:313:13:</a> PLR1714 Consider merging multiple comparisons. Use a `set` if the elements are hashable.
+ <a href='https://github.com/apache/airflow/blob/588d4b27248470f2e6cab8174e118026ef80f59e/airflow/providers/teradata/operators/teradata_compute_cluster.py#L122'>airflow/providers/teradata/operators/teradata_compute_cluster.py:122:13:</a> PLR1714 Consider merging multiple comparisons: `self.compute_profile_name in ("None", "")`. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/1e412a871174b1ab632ca1c95c5df5c39c20ecbf/superset/utils/core.py#L311'>superset/utils/core.py:311:13:</a> PLR1714 Consider merging multiple comparisons: `standalone_param not in ("false", "0")`. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L280'>src/bokeh/core/has_props.py:280:24:</a> PLR1714 Consider merging multiple comparisons: `head in ("bokeh", "__main__")`. Use a `set` if the elements are hashable.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_layouts.py#L292'>tests/unit/bokeh/test_layouts.py:292:12:</a> PLR1714 Consider merging multiple comparisons: `t5 not in (save0, save1)`. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/fc8fc82233e7910f5748acbc24ea6df77f4a233a/pandas/core/dtypes/astype.py#L128'>pandas/core/dtypes/astype.py:128:8:</a> PLR1714 Consider merging multiple comparisons: `object in (arr.dtype, dtype)`. Use a `set` if the elements are hashable.
+ <a href='https://github.com/pandas-dev/pandas/blob/fc8fc82233e7910f5748acbc24ea6df77f4a233a/pandas/core/nanops.py#L460'>pandas/core/nanops.py:460:13:</a> PLR1714 Consider merging multiple comparisons: `values.dtype not in (object, bool)`. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/c615022dcbb55c955bd7536dfb44758cf1d8d75c/rotkehlchen/__main__.py#L22'>rotkehlchen/__main__.py:22:12:</a> PLR1714 Consider merging multiple comparisons: `e.code in (0, 2)`. Use a `set` if the elements are hashable.
+ <a href='https://github.com/rotki/rotki/blob/c615022dcbb55c955bd7536dfb44758cf1d8d75c/rotkehlchen/chain/evm/decoding/curve/decoder.py#L542'>rotkehlchen/chain/evm/decoding/curve/decoder.py:542:18:</a> PLR1714 Consider merging multiple comparisons: `event.asset in (self.native_currency, sold_asset)`. Use a `set` if the elements are hashable.
+ <a href='https://github.com/rotki/rotki/blob/c615022dcbb55c955bd7536dfb44758cf1d8d75c/rotkehlchen/chain/evm/decoding/curve/decoder.py#L554'>rotkehlchen/chain/evm/decoding/curve/decoder.py:554:18:</a> PLR1714 Consider merging multiple comparisons. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/97753fe3b03f2fb920020e733aa43a6d108bbfd1/zerver/webhooks/opbeat/view.py#L82'>zerver/webhooks/opbeat/view.py:82:16:</a> PLR1714 Consider merging multiple comparisons: `key_raw not in ("html_url", "subject")`. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/wntrblm/nox">wntrblm/nox</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/wntrblm/nox/blob/7a77e805c6cc7a8d2b65467683da14c2b376e35a/nox/manifest.py#L322'>nox/manifest.py:322:16:</a> PLR1714 Consider merging multiple comparisons: `session in (s, s.name)`. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1714 | 12 | 12 | 0 | 0 | 0 |

</p>
</details>




---

_@dhruvmanila reviewed on 2024-07-18 06:28_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLR1714_repeated_equality_comparison.py.snap`:185 on 2024-07-18 06:28_

This seems incorrect? Or, is this expected? Other snapshot updates are also similar.

---

_@dhruvmanila reviewed on 2024-07-18 06:41_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/repeated_equality_comparison.rs`:120 on 2024-07-18 06:41_

The value of `last` needs to be updated before the `continue` in the inner condition to avoid the bug as mentioned in my other comment.

---

_@dhruvmanila reviewed on 2024-07-18 06:44_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/repeated_equality_comparison.rs`:120 on 2024-07-18 06:44_

```diff
diff --git a/crates/ruff_linter/src/rules/pylint/rules/repeated_equality_comparison.rs b/crates/ruff_linter/src/rules/pylint/rules/repeated_equalit>
index db2d9d7e5..adb9544c3 100644
--- a/crates/ruff_linter/src/rules/pylint/rules/repeated_equality_comparison.rs
+++ b/crates/ruff_linter/src/rules/pylint/rules/repeated_equality_comparison.rs
@@ -109,15 +109,13 @@ pub(crate) fn repeated_equality_comparison(checker: &mut Checker, bool_op: &ast:
         let mut sequences: Vec<(Vec<usize>, Vec<&Expr>)> = Vec::new();
         let mut last = None;
         for (index, comparator) in indices.iter().zip(comparators.iter()) {
-            if let Some(last) = last {
-                if last + 1 == *index {
-                    let (indices, comparators) = sequences.last_mut().unwrap();
-                    indices.push(*index);
-                    comparators.push(*comparator);
-                    continue;
-                }
+            if last.is_some_and(|last| last + 1 == *index) {
+                let (indices, comparators) = sequences.last_mut().unwrap();
+                indices.push(*index);
+                comparators.push(*comparator);
+            } else {
+                sequences.push((vec![*index], vec![*comparator]));
             }
-            sequences.push((vec![*index], vec![*comparator]));
             last = Some(*index);
         }
```

---

_@charliermarsh reviewed on 2024-07-18 14:55_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLR1714_repeated_equality_comparison.py.snap`:185 on 2024-07-18 14:55_

Oops, thanks.

---

_Review requested from @dhruvmanila by @charliermarsh on 2024-07-18 15:00_

---

_@dhruvmanila approved on 2024-07-18 15:13_

---

_Merged by @charliermarsh on 2024-07-18 15:16_

---

_Closed by @charliermarsh on 2024-07-18 15:16_

---

_Branch deleted on 2024-07-18 15:16_

---
