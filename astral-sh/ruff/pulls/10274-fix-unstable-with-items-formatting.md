```yaml
number: 10274
title: Fix unstable with-items formatting
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: fix-with-item-parenthesizing
created_at: 2024-03-07T15:05:03Z
updated_at: 2024-03-09T01:26:13Z
url: https://github.com/astral-sh/ruff/pull/10274
synced_at: 2026-01-10T22:47:01Z
```

# Fix unstable with-items formatting

---

_Pull request opened by @MichaReiser on 2024-03-07 15:05_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/10267

The issue with the current formatting is that the formatter flips between the `SingleParenthesizedContextManager`  and `ParenthesizeIfExpands` or `SingleWithTarget` because the layouts use incompatible formatting ( `SingleParenthesizedContextManager`: `maybe_parenthesize_expression(context)` vs `ParenthesizeIfExpands`: `parenthesize_if_expands(item)`, `SingleWithTarget`: `optional_parentheses(item)`. 

The fix is to ensure that the layouts between which the formatter flips when adding or removing parentheses are the same. I do this by introducing a new `SingleWithoutTarget` layout that uses the same formatting as `SingleParenthesizedContextManager` if it has no target and prefer `SingleWithoutTarget` over using `ParenthesizeIfExpands` or `SingleWithTarget`. 

## Formatting change

The downside is that we now use `maybe_parenthesize_expression` over `parenthesize_if_expands` for expressions where `can_omit_optional_parentheses` returns `false`. This can lead to stable formatting changes. I only found one formatting change in our ecosystem check and, unfortunately, this is necessary to fix the instability (and instability fixes are okay to have as part of minor changes according to our versioning policy)

The benefit of the change is that `with` items with a single context manager and without a target are now formatted identically to how the same expression would be formatted in other clause headers.

## Test Plan

I ran the ecosystem check locally

---

_Label `bug` added by @MichaReiser on 2024-03-07 15:21_

---

_Label `formatter` added by @MichaReiser on 2024-03-07 15:21_

---

_Comment by @github-actions[bot] on 2024-03-07 15:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+2 -2 lines in 1 file in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -2 lines across 1 file)</summary>
<p>

<a href='https://github.com/rotki/rotki/blob/0e9b5bc8521c98ece568854ed7f45aa8c6c9411f/rotkehlchen/rotkehlchen.py#L1213'>rotkehlchen/rotkehlchen.py~L1213</a>
```diff
         if oracle != HistoricalPriceOracle.CRYPTOCOMPARE:
             return  # only for cryptocompare for now
 
-        with (
-            contextlib.suppress(UnknownAsset)
+        with contextlib.suppress(
+            UnknownAsset
         ):  # if suppress -> assets are not crypto or fiat, so we can't query cryptocompare  # noqa: E501
             self.cryptocompare.create_cache(
                 from_asset=from_asset,
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+2 -2 lines in 1 file in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/rotki/rotki/blob/0e9b5bc8521c98ece568854ed7f45aa8c6c9411f/rotkehlchen/rotkehlchen.py#L1211'>rotkehlchen/rotkehlchen.py~L1211</a>
```diff
         if oracle != HistoricalPriceOracle.CRYPTOCOMPARE:
             return  # only for cryptocompare for now
 
-        with (
-            contextlib.suppress(UnknownAsset)
+        with contextlib.suppress(
+            UnknownAsset
         ):  # if suppress -> assets are not crypto or fiat, so we can't query cryptocompare  # noqa: E501
             self.cryptocompare.create_cache(
                 from_asset=from_asset,
```

</p>
</details>




---

_Comment by @aneeshusa on 2024-03-07 15:36_

I'm traveling today but am happy to give this a try tomorrow! Amazed how quick you put this together, thank you!

---

_@MichaReiser reviewed on 2024-03-07 16:58_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/with.py`:16 on 2024-03-07 16:58_

TODO: revert formatting changes

---

_@MichaReiser reviewed on 2024-03-07 17:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/arguments.rs`:208 on 2024-03-07 17:52_

I noticed that we pass `context` and `options`. Passing the `context` alone is sufficient because the options can be accessed by `context.options()`.

---

_@MichaReiser reviewed on 2024-03-07 17:53_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_with.rs`:141 on 2024-03-07 17:53_

TODO: It might be worth to split the refactor into its own PR

---

_Comment by @charliermarsh on 2024-03-07 22:13_

Thanks @aneeshusa :wave:

---

_Marked ready for review by @MichaReiser on 2024-03-08 11:33_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-03-08 16:12_

---

_@aneeshusa reviewed on 2024-03-08 23:25_

Can't do a GitHub approval but tested this and this fixes both an isolated repro and the monorepo at $WORK! Would love to see this in a release.

---

_@charliermarsh approved on 2024-03-08 23:42_

---

_Merged by @charliermarsh on 2024-03-08 23:48_

---

_Closed by @charliermarsh on 2024-03-08 23:48_

---

_Branch deleted on 2024-03-08 23:48_

---

_Comment by @charliermarsh on 2024-03-09 01:26_

Just shipped this in v0.3.2 :)

---
