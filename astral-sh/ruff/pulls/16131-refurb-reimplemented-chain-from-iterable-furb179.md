```yaml
number: 16131
title: "[`refurb`] Reimplemented `chain.from_iterable()` (`FURB179`)"
type: pull_request
state: open
author: InSyncWithFoo
labels:
  - rule
  - needs-decision
assignees: []
base: main
head: FURB179
created_at: 2025-02-13T06:44:19Z
updated_at: 2025-02-26T14:44:57Z
url: https://github.com/astral-sh/ruff/pull/16131
synced_at: 2026-01-12T15:55:53Z
```

# [`refurb`] Reimplemented `chain.from_iterable()` (`FURB179`)

---

_@InSyncWithFoo_

## Summary

Part of #1348.

[`FURB179`](https://github.com/dosisod/refurb/blob/7649900948c8a65296ae6efc9d8ced0bc1a54a7f/refurb/checks/itertools/use_chain_from_iterable.py) reports the following patterns and offers to replace them with `chain.from_iterable()`:

* `functools.chain(*a)`
* `functools.reduce(op, a, start)` where `op` is either `operator.add` or `operator.concat` and `start` is `[]`, `()`, `list()`, `tuple[str, int]()` and the like.
* `[a for b in c for a in b]` and other comprehensions

Unlike the upstream rule, it does not report `sum(a, [])`, as this is already reported by [`RUF017`](https://docs.astral.sh/ruff/rules/quadratic-list-summation/). `RUF017` (a stable rule) should eventually be merged into this rule.

Rule logic and tests were adopted from that of the upstream rule.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-13 06:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+23 -0 violations, +0 -0 fixes in 9 projects; 46 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/d01a1431616d29bcc26ee29e03d476ba06557a8e/airflow/dag_processing/collection.py#L787'>airflow/dag_processing/collection.py:787:44:</a> FURB179 Use `chain.from_iterable()` instead
+ <a href='https://github.com/apache/airflow/blob/d01a1431616d29bcc26ee29e03d476ba06557a8e/airflow/lineage/__init__.py#L141'>airflow/lineage/__init__.py:141:36:</a> FURB179 Use `chain.from_iterable()` instead
+ <a href='https://github.com/apache/airflow/blob/d01a1431616d29bcc26ee29e03d476ba06557a8e/airflow/utils/helpers.py#L171'>airflow/utils/helpers.py:171:12:</a> FURB179 Use `chain.from_iterable()` instead
+ <a href='https://github.com/apache/airflow/blob/d01a1431616d29bcc26ee29e03d476ba06557a8e/providers/fab/tests/unit/fab/auth_manager/test_fab_auth_manager.py#L180'>providers/fab/tests/unit/fab/auth_manager/test_fab_auth_manager.py:180:13:</a> FURB179 Use `chain.from_iterable()` instead
+ <a href='https://github.com/apache/airflow/blob/d01a1431616d29bcc26ee29e03d476ba06557a8e/providers/google/src/airflow/providers/google/ads/hooks/ads.py#L318'>providers/google/src/airflow/providers/google/ads/hooks/ads.py:318:20:</a> FURB179 Use `chain.from_iterable()` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/6177b9f9ab81e8750292ef5951115e5135909c9e/libs/core/langchain_core/output_parsers/list.py#L173'>libs/core/langchain_core/output_parsers/list.py:173:20:</a> FURB179 Use `chain.from_iterable()` instead
+ <a href='https://github.com/langchain-ai/langchain/blob/6177b9f9ab81e8750292ef5951115e5135909c9e/libs/core/tests/unit_tests/fake/callbacks.py#L275'>libs/core/tests/unit_tests/fake/callbacks.py:275:62:</a> FURB179 Use `chain.from_iterable()` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/1b0fe23974d7d2f3a1df75b23d9bf06b83f5132f/src/latch/utils.py#L113'>src/latch/utils.py:113:44:</a> FURB179 Use `chain.from_iterable()` instead
+ <a href='https://github.com/latchbio/latch/blob/1b0fe23974d7d2f3a1df75b23d9bf06b83f5132f/src/latch/utils.py#L118'>src/latch/utils.py:118:45:</a> FURB179 Use `chain.from_iterable()` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/e0e3397747ba8a83d7499344612cd3c3a9fa23c3/tests/units/components/test_component.py#L1482'>tests/units/components/test_component.py:1482:27:</a> FURB179 Use `chain.from_iterable()` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/b712887b144eab972e54393c918303296fb0d5ae/rotkehlchen/exchanges/coinbaseprime.py#L582'>rotkehlchen/exchanges/coinbaseprime.py:582:21:</a> FURB179 Use `chain.from_iterable()` instead
+ <a href='https://github.com/rotki/rotki/blob/b712887b144eab972e54393c918303296fb0d5ae/tools/profiling/graph.py#L208'>tools/profiling/graph.py:208:23:</a> FURB179 Use `chain.from_iterable()` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/e9ab153a3769d71ceaef108ba6259ece2de70676/src/scikit_build_core/builder/builder.py#L155'>src/scikit_build_core/builder/builder.py:155:13:</a> FURB179 Use `chain.from_iterable()` instead
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/e9ab153a3769d71ceaef108ba6259ece2de70676/src/scikit_build_core/builder/builder.py#L163'>src/scikit_build_core/builder/builder.py:163:13:</a> FURB179 Use `chain.from_iterable()` instead
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/e9ab153a3769d71ceaef108ba6259ece2de70676/tests/conftest.py#L392'>tests/conftest.py:392:13:</a> FURB179 Use `chain.from_iterable()` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/2c8d74735a598c2b36a53ef20f50d2eef0babfc0/zerver/actions/streams.py#L1044'>zerver/actions/streams.py:1044:26:</a> FURB179 Use `chain.from_iterable()` instead
+ <a href='https://github.com/zulip/zulip/blob/2c8d74735a598c2b36a53ef20f50d2eef0babfc0/zerver/tests/test_push_notifications.py#L5136'>zerver/tests/test_push_notifications.py:5136:26:</a> FURB179 Use `chain.from_iterable()` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/d5ca605c03cbbb94502dfe1d7e807f7aa32aa02a/indico/modules/categories/controllers/display.py#L521'>indico/modules/categories/controllers/display.py:521:42:</a> FURB179 Use `chain.from_iterable()` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/7368ab84abfeddf960b7315a12a565de948615de/astropy/io/ascii/daophot.py#L92'>astropy/io/ascii/daophot.py:92:35:</a> FURB179 Use `chain.from_iterable()` instead
+ <a href='https://github.com/astropy/astropy/blob/7368ab84abfeddf960b7315a12a565de948615de/astropy/modeling/projections.py#L81'>astropy/modeling/projections.py:81:69:</a> FURB179 Use `chain.from_iterable()` instead
+ <a href='https://github.com/astropy/astropy/blob/7368ab84abfeddf960b7315a12a565de948615de/astropy/nddata/blocks.py#L92'>astropy/nddata/blocks.py:92:23:</a> FURB179 Use `chain.from_iterable()` instead
+ <a href='https://github.com/astropy/astropy/blob/7368ab84abfeddf960b7315a12a565de948615de/astropy/table/operations.py#L1455'>astropy/table/operations.py:1455:33:</a> FURB179 Use `chain.from_iterable()` instead
+ <a href='https://github.com/astropy/astropy/blob/7368ab84abfeddf960b7315a12a565de948615de/astropy/timeseries/periodograms/lombscargle/implementations/fastchi2_impl.py#L117'>astropy/timeseries/periodograms/lombscargle/implementations/fastchi2_impl.py:117:24:</a> FURB179 Use `chain.from_iterable()` instead
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB179 | 23 | 23 | 0 | 0 | 0 |

</p>
</details>




---

_@MichaReiser reviewed on 2025-02-13 08:47_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_chain_from_iterable.rs`:76 on 2025-02-13 08:47_

What's the motivation for using the &sect; sign here? And can't we just use the actual character §

---

_Comment by @MichaReiser on 2025-02-13 08:50_

Thanks. I'm not sure if the long-term goal should be to merge RUF017 into this rule. They seem to have different motivations (performance vs idiomatic code)

---

_Label `rule` added by @MichaReiser on 2025-02-13 08:50_

---

_Label `needs-decision` added by @MichaReiser on 2025-02-13 08:50_

---

_@InSyncWithFoo reviewed on 2025-02-13 14:09_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_chain_from_iterable.rs`:76 on 2025-02-13 14:09_

It's just a habit of mine. The actual character might not be well-displayed by some fonts, hence the HTML entity.

---

_@MichaReiser reviewed on 2025-02-13 14:11_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_chain_from_iterable.rs`:76 on 2025-02-13 14:11_

Okay, but what's the purpose of the sign in the first place?

---

_@InSyncWithFoo reviewed on 2025-02-13 14:18_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_chain_from_iterable.rs`:76 on 2025-02-13 14:18_

[`§` (section sign)](https://en.wikipedia.org/wiki/Section_sign) represents a reference to a section of a document. It's perhaps rather uncommon in everyday online conversation.

---

_@MichaReiser reviewed on 2025-02-13 14:29_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_chain_from_iterable.rs`:76 on 2025-02-13 14:29_

I think just `chain.from_iterable` is good enough here

---

_Comment by @Skylion007 on 2025-02-13 18:22_

Tested this on the PyTorch codebase, it doesn't handle list or set comprehensions correctly.

```diff
     def get_log_qnames(self) -> set[str]:
-        return {
-            qname
-            for qnames in self.log_alias_to_log_qnames.values()
-            for qname in qnames
-        }
+        return itertools.chain.from_iterable(qnames)
```
is obviously not a set (and the transformation is incorrect to boot!)

and 
```diff
index d5f4daf39d4..5ba751242ab 100644
--- a/torch/_inductor/codegen/cpp.py
+++ b/torch/_inductor/codegen/cpp.py
@@ -4895,7 +4895,7 @@ class CppScheduling(BaseScheduling):
                 )
                 kernel_group.finalize_kernel(
                     outer_fusion_cpp_kernel_proxy,
-                    [_node for _nodes in nodes_list for _node in _nodes],
+                    itertools.chain.from_iterable(_nodes),
                 )

             return True
```
is obviously not a list.

---

_Comment by @charliermarsh on 2025-02-13 18:27_

I'm not sure that the second one is necessarily _wrong_; it's just unsafe.

---

_Comment by @InSyncWithFoo on 2025-02-13 18:33_

@Skylion007 That was quite a bug I didn't notice. Should be easy to fix though. I'll get to that if this PR doesn't get rejected.

---

_Comment by @Skylion007 on 2025-02-14 14:48_

> I'm not sure that the second one is necessarily _wrong_; it's just unsafe.

Main issue is that it can break type checking as people often annotate methods take a List explicitly instead of an arbitrary Iterable or Sequence.

---

_Comment by @Skylion007 on 2025-02-14 14:50_

This is a pretty useful PR, I've been waiting for this rule for a while given how useful it is for ensuring efficient, readable code.

---

_Comment by @Skylion007 on 2025-02-21 14:33_

> I'm not sure that the second one is necessarily _wrong_; it's just unsafe.

To clarify further, it's a semantic change to the code because something that was a list is now just an iterable and that can cause issues since you can't call get or len on it. It would be proper if the itertools.chain iterable was wrapped in a list ctor.

---

_Comment by @InSyncWithFoo on 2025-02-21 15:14_

@Skylion007 The bug should be fixed now. Could you try again?

---

_Comment by @Skylion007 on 2025-02-23 15:44_

@InSyncWithFoo Still had issues with some comprehensions:

```python
     def get_log_qnames(self) -> set[str]:
-        return {
-            qname
-            for qnames in self.log_alias_to_log_qnames.values()
-            for qname in qnames
-        }
+        return set(itertools.chain.from_iterable(qnames))
```
^ Still did a wrong comprehension here.

~Also, would probably good to include an option asking if you want to actually unnest nested list comprehensions since I can imagine those might be faster / more readable in certain circmstances.~ Ah, interesting. Counter to my assumption, chain from iterable is faster even in the nested list comprehension setting.

---

_Comment by @Skylion007 on 2025-02-23 17:24_

Okay, so that does seem significantly better with the latest fix. Also, we should probably consider updating C4 rules at some point to have chain.fromiterable be a useless iterable to list/tuple conversion. `itertools.chain.from_iterable(tuple(iter))` isn't particularly useful as far as I can tell.

---

_Comment by @Skylion007 on 2025-02-23 17:27_

The operator.add transform is still unsafe though because it can change the return type of functions. After all, itertools.chain returns an iterator, where operator.add usually concatenates it to a list or tuple depending on the base case. Having a config flag to enable this behavior of the rule would be helpful.

---

_Comment by @Skylion007 on 2025-02-23 17:33_

it also seems to be assuming every operator.add reduction is a list/tuple reduction, when some could have been an integer, float or other reduction. For instance, it suggested this is sympy:
```
-                return reduce(add, list(map(_rebuild, expr.args)))
+                return chain.from_iterable(list(map(_rebuild, expr.args)))
```
When I don't think it statically inferred that expr.args is not a list of lists, but sometime a list of symbolic numbers. So it has a lot of false negatives. Sometimes an operataor.add reduction is just a reduction of numbers inside of a list; and therefore valid.

---

_Review comment by @Skylion007 on `crates/ruff_linter/resources/test/fixtures/refurb/FURB179_0.py`:31 on 2025-02-23 17:35_

This is only valid if you know e is a list of list/tuple, add a test where it's not (like it's just a list of numbers). This should be no error.

---

_@Skylion007 reviewed on 2025-02-23 17:36_

---

_Comment by @InSyncWithFoo on 2025-02-23 18:17_

`chain.from_iterable(tuple(iter))` makes a copy of/consumes `iter`. Consider:

```pycon
>>> a = [[1], [2], [3]]
>>> c = chain.from_iterable(a)
>>> a.append([4])
>>> [*c]
[1, 2, 3, 4]
```

```pycon
>>> a = [[1], [2], [3]]
>>> c = chain.from_iterable(tuple(a))
>>> a.append([4])
>>> [*c]
[1, 2, 3]
```

---

_@InSyncWithFoo reviewed on 2025-02-23 18:19_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/refurb/FURB179_0.py`:31 on 2025-02-23 18:19_

That's true, but I would rather save this for a follow-up PR. This PR is already so big, it risks not being reviewed.

---

_@Skylion007 reviewed on 2025-02-24 14:19_

---

_Review comment by @Skylion007 on `crates/ruff_linter/resources/test/fixtures/refurb/FURB179_0.py`:31 on 2025-02-24 14:19_

That the last change needed to make
it useful and there should be builtin utilities to determine is something is a list/tuple in ruff

---

_@InSyncWithFoo reviewed on 2025-02-24 14:27_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/refurb/FURB179_0.py`:31 on 2025-02-24 14:27_

There are indeed [a few functions](https://github.com/astral-sh/ruff/blob/68991d09a8f64d1c155cfc5d96f9220daee07511/crates/ruff_python_semantic/src/analyze/typing.rs#L964-L966) to check that a variable is a list or tuple, but not a <em>nested</em> list or tuple. [`RUF017`](https://docs.astral.sh/ruff/rules/quadratic-list-summation/) only reports when the third argument is an emty list; perhaps `FURB179` could copy that?

---

_@Skylion007 reviewed on 2025-02-24 14:58_

---

_Review comment by @Skylion007 on `crates/ruff_linter/resources/test/fixtures/refurb/FURB179_0.py`:31 on 2025-02-24 14:58_

Empty list, empty tuple, and empty set would all be valid, right? It's better to be too conservative vs too aggressive. Surprised there isn't a nested utility for this.

---

_@Skylion007 reviewed on 2025-02-24 15:00_

---

_Review comment by @Skylion007 on `crates/ruff_linter/resources/test/fixtures/refurb/FURB179_0.py`:31 on 2025-02-24 15:00_

We can add a todo comment for the more complicated uses cases later.

---

_Comment by @Skylion007 on 2025-02-26 14:33_

PR description should be updated. Also surprised there isn't more logic that can be extracted and shared with the relevant reduce RUF rule or any C4 utilities for rewriting comprehensions.

---
