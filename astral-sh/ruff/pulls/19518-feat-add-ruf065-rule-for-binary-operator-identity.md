```yaml
number: 19518
title: "feat: Add `RUF065` rule for binary operator identity simplification"
type: pull_request
state: closed
author: danparizher
labels:
  - rule
assignees: []
draft: true
base: main
head: binary-operator-identity
created_at: 2025-07-24T00:59:47Z
updated_at: 2025-08-20T21:02:54Z
url: https://github.com/astral-sh/ruff/pull/19518
synced_at: 2026-01-12T15:56:41Z
```

# feat: Add `RUF065` rule for binary operator identity simplification

---

_@danparizher_

## Summary

This PR implements `RUF065` (`BinaryOperatorIdentity`) that detects binary operations between a value and itself and suggests simplifications to their known identities. This rule is inspired by similar functionality in Sourcery and contributes to issue #11060.

## What it does

The rule identifies binary operations where both operands are identical and can be simplified to known mathematical identities:

- `x | x` → `x` (bitwise OR identity)
- `x & x` → `x` (bitwise AND identity) 
- `x ^ x` → `0` (bitwise XOR identity)
- `x - x` → `0` (subtraction identity)
- `x / x` → `1` (division identity)
- `x // x` → `1` (floor division identity)
- `x % x` → `0` (modulo identity)

@ntBre, @MichaReiser, I wanted to draft this PR as a proof of concept of what this may look like as a custom `RUF` rule implementation. Please critique and comment as you see fit! I'm sure there are many details to discuss in terms of where we want the rule to live, semantics, opinionatedness, etc. Thanks!

---

_Comment by @github-actions[bot] on 2025-07-24 01:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+104 -0 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/75a944ba641ee88e21d000ad9ce53e50160f5a3c/ibis/backends/tests/test_array.py#L668'>ibis/backends/tests/test_array.py:668:24:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/ibis-project/ibis/blob/75a944ba641ee88e21d000ad9ce53e50160f5a3c/ibis/backends/tests/test_generic.py#L332'>ibis/backends/tests/test_generic.py:332:23:</a> RUF065 Binary operation `&` between a value and itself can be simplified to `t.bool_col`
+ <a href='https://github.com/ibis-project/ibis/blob/75a944ba641ee88e21d000ad9ce53e50160f5a3c/ibis/backends/tests/test_generic.py#L333'>ibis/backends/tests/test_generic.py:333:24:</a> RUF065 Binary operation `&` between a value and itself can be simplified to `df.bool_col`
+ <a href='https://github.com/ibis-project/ibis/blob/75a944ba641ee88e21d000ad9ce53e50160f5a3c/ibis/backends/tests/test_generic.py#L342'>ibis/backends/tests/test_generic.py:342:23:</a> RUF065 Binary operation `^` between a value and itself can be simplified to `0`
+ <a href='https://github.com/ibis-project/ibis/blob/75a944ba641ee88e21d000ad9ce53e50160f5a3c/ibis/backends/tests/test_generic.py#L343'>ibis/backends/tests/test_generic.py:343:24:</a> RUF065 Binary operation `^` between a value and itself can be simplified to `0`
+ <a href='https://github.com/ibis-project/ibis/blob/75a944ba641ee88e21d000ad9ce53e50160f5a3c/ibis/expr/operations/tests/test_rewrites.py#L100'>ibis/expr/operations/tests/test_rewrites.py:100:10:</a> RUF065 Binary operation `/` between a value and itself can be simplified to `1`
+ <a href='https://github.com/ibis-project/ibis/blob/75a944ba641ee88e21d000ad9ce53e50160f5a3c/ibis/expr/operations/tests/test_rewrites.py#L101'>ibis/expr/operations/tests/test_rewrites.py:101:10:</a> RUF065 Binary operation `/` between a value and itself can be simplified to `1`
+ <a href='https://github.com/ibis-project/ibis/blob/75a944ba641ee88e21d000ad9ce53e50160f5a3c/ibis/expr/operations/tests/test_rewrites.py#L102'>ibis/expr/operations/tests/test_rewrites.py:102:17:</a> RUF065 Binary operation `/` between a value and itself can be simplified to `1`
+ <a href='https://github.com/ibis-project/ibis/blob/75a944ba641ee88e21d000ad9ce53e50160f5a3c/ibis/expr/operations/tests/test_rewrites.py#L103'>ibis/expr/operations/tests/test_rewrites.py:103:17:</a> RUF065 Binary operation `/` between a value and itself can be simplified to `1`
+ <a href='https://github.com/ibis-project/ibis/blob/75a944ba641ee88e21d000ad9ce53e50160f5a3c/ibis/expr/operations/tests/test_rewrites.py#L104'>ibis/expr/operations/tests/test_rewrites.py:104:19:</a> RUF065 Binary operation `/` between a value and itself can be simplified to `1`
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+62 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/asv_bench/benchmarks/arithmetic.py#L378'>asv_bench/benchmarks/arithmetic.py:378:27:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/asv_bench/benchmarks/arithmetic.py#L382'>asv_bench/benchmarks/arithmetic.py:382:9:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arithmetic/test_datetime64.py#L1045'>pandas/tests/arithmetic/test_datetime64.py:1045:20:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arithmetic/test_datetime64.py#L1060'>pandas/tests/arithmetic/test_datetime64.py:1060:20:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arithmetic/test_datetime64.py#L2157'>pandas/tests/arithmetic/test_datetime64.py:2157:20:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arithmetic/test_datetime64.py#L2177'>pandas/tests/arithmetic/test_datetime64.py:2177:18:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arithmetic/test_datetime64.py#L2180'>pandas/tests/arithmetic/test_datetime64.py:2180:18:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arithmetic/test_numeric.py#L1446'>pandas/tests/arithmetic/test_numeric.py:1446:31:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arithmetic/test_numeric.py#L1447'>pandas/tests/arithmetic/test_numeric.py:1447:21:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arithmetic/test_numeric.py#L530'>pandas/tests/arithmetic/test_numeric.py:530:18:</a> RUF065 Binary operation `/` between a value and itself can be simplified to `1`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arithmetic/test_numeric.py#L825'>pandas/tests/arithmetic/test_numeric.py:825:32:</a> RUF065 Binary operation `/` between a value and itself can be simplified to `1`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arithmetic/test_object.py#L325'>pandas/tests/arithmetic/test_object.py:325:13:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arithmetic/test_period.py#L1328'>pandas/tests/arithmetic/test_period.py:1328:20:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arithmetic/test_timedelta64.py#L374'>pandas/tests/arithmetic/test_timedelta64.py:374:18:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arithmetic/test_timedelta64.py#L431'>pandas/tests/arithmetic/test_timedelta64.py:431:18:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arithmetic/test_timedelta64.py#L447'>pandas/tests/arithmetic/test_timedelta64.py:447:18:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arithmetic/test_timedelta64.py#L576'>pandas/tests/arithmetic/test_timedelta64.py:576:20:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arithmetic/test_timedelta64.py#L587'>pandas/tests/arithmetic/test_timedelta64.py:587:15:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arrays/integer/test_arithmetic.py#L330'>pandas/tests/arrays/integer/test_arithmetic.py:330:45:</a> RUF065 Binary operation `&` between a value and itself can be simplified to `4`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arrays/integer/test_arithmetic.py#L334'>pandas/tests/arrays/integer/test_arithmetic.py:334:45:</a> RUF065 Binary operation `^` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/arrays/test_datetimelike.py#L823'>pandas/tests/arrays/test_datetimelike.py:823:59:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/dtypes/test_missing.py#L206'>pandas/tests/dtypes/test_missing.py:206:53:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/extension/base/dim2.py#L287'>pandas/tests/extension/base/dim2.py:287:57:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/extension/test_arrow.py#L1258'>pandas/tests/extension/test_arrow.py:1258:46:</a> RUF065 Binary operation `&` between a value and itself can be simplified to `4`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/extension/test_arrow.py#L1262'>pandas/tests/extension/test_arrow.py:1262:46:</a> RUF065 Binary operation `^` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/frame/methods/test_diff.py#L64'>pandas/tests/frame/methods/test_diff.py:64:20:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/frame/methods/test_diff.py#L98'>pandas/tests/frame/methods/test_diff.py:98:20:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/frame/methods/test_values.py#L236'>pandas/tests/frame/methods/test_values.py:236:15:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/pandas-dev/pandas/blob/78ec27668163808b3488da6ed2c64d0b8e9bfbc8/pandas/tests/frame/methods/test_values.py#L237'>pandas/tests/frame/methods/test_values.py:237:15:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
... 33 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-trio/trio/blob/732f15744752ccfb8603cdae3f3f49eca7f39df6/src/trio/_tests/type_tests/path.py#L17'>src/trio/_tests/type_tests/path.py:17:17:</a> RUF065 Binary operation `/` between a value and itself can be simplified to `1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/d1e931a9dac3c14833d62a6263478a64ea0c634c/testing/code/test_excinfo.py#L427'>testing/code/test_excinfo.py:427:13:</a> RUF065 Binary operation `//` between a value and itself can be simplified to `1`
+ <a href='https://github.com/pytest-dev/pytest/blob/d1e931a9dac3c14833d62a6263478a64ea0c634c/testing/test_runner.py#L549'>testing/test_runner.py:549:45:</a> RUF065 Binary operation `/` between a value and itself can be simplified to `1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+27 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/857fbc9300eb4998b2e56e9c75cae8c82f6d55e5/astropy/coordinates/tests/test_representation_arithmetic.py#L1475'>astropy/coordinates/tests/test_representation_arithmetic.py:1475:9:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/astropy/astropy/blob/857fbc9300eb4998b2e56e9c75cae8c82f6d55e5/astropy/coordinates/tests/test_representation_arithmetic.py#L1481'>astropy/coordinates/tests/test_representation_arithmetic.py:1481:9:</a> RUF065 Binary operation `/` between a value and itself can be simplified to `1`
+ <a href='https://github.com/astropy/astropy/blob/857fbc9300eb4998b2e56e9c75cae8c82f6d55e5/astropy/coordinates/tests/test_representation_arithmetic.py#L215'>astropy/coordinates/tests/test_representation_arithmetic.py:215:14:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/astropy/astropy/blob/857fbc9300eb4998b2e56e9c75cae8c82f6d55e5/astropy/coordinates/tests/test_representation_arithmetic.py#L247'>astropy/coordinates/tests/test_representation_arithmetic.py:247:14:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/astropy/astropy/blob/857fbc9300eb4998b2e56e9c75cae8c82f6d55e5/astropy/coordinates/tests/test_representation_arithmetic.py#L692'>astropy/coordinates/tests/test_representation_arithmetic.py:692:19:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/astropy/astropy/blob/857fbc9300eb4998b2e56e9c75cae8c82f6d55e5/astropy/coordinates/tests/test_representation_arithmetic.py#L862'>astropy/coordinates/tests/test_representation_arithmetic.py:862:19:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/astropy/astropy/blob/857fbc9300eb4998b2e56e9c75cae8c82f6d55e5/astropy/io/misc/tests/test_yaml.py#L246'>astropy/io/misc/tests/test_yaml.py:246:10:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/astropy/astropy/blob/857fbc9300eb4998b2e56e9c75cae8c82f6d55e5/astropy/modeling/tests/test_compound.py#L214'>astropy/modeling/tests/test_compound.py:214:9:</a> RUF065 Binary operation `&` between a value and itself can be simplified to `G`
+ <a href='https://github.com/astropy/astropy/blob/857fbc9300eb4998b2e56e9c75cae8c82f6d55e5/astropy/modeling/tests/test_compound.py#L235'>astropy/modeling/tests/test_compound.py:235:9:</a> RUF065 Binary operation `-` between a value and itself can be simplified to `0`
+ <a href='https://github.com/astropy/astropy/blob/857fbc9300eb4998b2e56e9c75cae8c82f6d55e5/astropy/modeling/tests/test_compound.py#L237'>astropy/modeling/tests/test_compound.py:237:9:</a> RUF065 Binary operation `/` between a value and itself can be simplified to `1`
+ <a href='https://github.com/astropy/astropy/blob/857fbc9300eb4998b2e56e9c75cae8c82f6d55e5/astropy/modeling/tests/test_compound.py#L403'>astropy/modeling/tests/test_compound.py:403:9:</a> RUF065 Binary operation `&` between a value and itself can be simplified to `g1`
+ <a href='https://github.com/astropy/astropy/blob/857fbc9300eb4998b2e56e9c75cae8c82f6d55e5/astropy/modeling/tests/test_models.py#L821'>astropy/modeling/tests/test_models.py:821:9:</a> RUF065 Binary operation `&` between a value and itself can be simplified to `models.Polynomial2D(2)`
... 15 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF065 | 104 | 104 | 0 | 0 | 0 |

</p>
</details>




---

_Review requested from @ntBre by @ntBre on 2025-07-24 13:06_

---

_Label `rule` added by @ntBre on 2025-07-25 22:00_

---

_Comment by @MichaReiser on 2025-07-28 09:59_

I haven't looked at the implementation. The rule itself seems reasonable as a Ruff rule because it is about simplification, or even catching potential bugs. 

What's less clear to me is if the rule should provide a fix because it's not clear to me whether `x & x` was intentional or if they mistyped the variable.

---

_Comment by @danparizher on 2025-07-28 18:34_

I think we should provide the fixes since these are guaranteed mathematical identities (unlike other simplifications that might change behavior), but add a note in the rule description that developers should verify this was their intent.

We could also consider a separate rule for catching suspicious identity operations without fixes, if that makes more sense.

---

_Comment by @ntBre on 2025-07-28 20:37_

I think Micha's point was more that the user probably meant to write something like `x & y` rather than just `x`. The issue isn't that our fix would change behavior but that we could silently preserve the wrong behavior. Instead we should alert the user to the issue so that they can decide what it's supposed to say.

An unsafe or display-only fix might be a good compromise here so that users can apply the fix in an editor, for example, but we don't silently perform the fix without alerting them.

I don't think documenting the issue would be enough when the fix is marked as safe.

---

_Comment by @danparizher on 2025-07-30 02:05_

Got it. One thing I'm still trying to figure out is the cases for `|` and `%`. Looking into why violations aren't being reported for those.

---

_Comment by @MichaReiser on 2025-08-18 07:41_

@danparizher I'll convert this PR to draft. It makes it easier for us to know which PR's are ready to have a look. Let us know if you need any help

---

_Converted to draft by @MichaReiser on 2025-08-18 07:41_

---

_Comment by @github-actions[bot] on 2025-08-20 20:43_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-20 20:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Closed by @danparizher on 2025-08-20 21:02_

---
