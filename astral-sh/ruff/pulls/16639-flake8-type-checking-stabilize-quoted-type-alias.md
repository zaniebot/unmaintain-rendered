```yaml
number: 16639
title: "[`flake8-type-checking`] Stabilize `quoted-type-alias` (`TC008`)"
type: pull_request
state: closed
author: ntBre
labels:
  - rule
assignees: []
base: micha/ruff-0.10
head: brent/tc008-0.10
created_at: 2025-03-11T17:29:29Z
updated_at: 2025-05-01T15:32:03Z
url: https://github.com/astral-sh/ruff/pull/16639
synced_at: 2026-01-12T15:55:55Z
```

# [`flake8-type-checking`] Stabilize `quoted-type-alias` (`TC008`)

---

_@ntBre_

Summary
--

Stabilizes TC008. The test was already in the right place.

Test Plan
--

No open issues or PRs.

---

_Label `rule` added by @ntBre on 2025-03-11 17:29_

---

_Added to milestone `v0.10` by @ntBre on 2025-03-11 17:29_

---

_Comment by @codspeed-hq[bot] on 2025-03-11 17:34_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Ftc008-0.10)

### Merging #16639 will **degrade performances by 12.88%**

<sub>Comparing <code>brent/tc008-0.10</code> (174758b) with <code>micha/ruff-0.10</code> (5ec7c13)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/brent%2Ftc008-0.10)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.8 ms | 5.5 ms | -12.88% |


---

_Comment by @github-actions[bot] on 2025-03-11 17:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+9 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/f6ea1ca4959617b84a7f01dd5d1237c5cf22ab95/ibis/common/typing.py#L28'>ibis/common/typing.py:28:47:</a> TC008 [*] Remove quotes from type alias
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch/ldata/_transfer/upload.py#L30'>src/latch/ldata/_transfer/upload.py:30:32:</a> TC008 [*] Remove quotes from type alias
+ <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch/ldata/_transfer/upload.py#L31'>src/latch/ldata/_transfer/upload.py:31:35:</a> TC008 [*] Remove quotes from type alias
+ <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch_cli/snakemake/config/utils.py#L13'>src/latch_cli/snakemake/config/utils.py:13:33:</a> TC008 [*] Remove quotes from type alias
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/48e8250e2836fc39472518b7bf5d69da543eb768/analytics/views/stats.py#L341'>analytics/views/stats.py:341:14:</a> TC008 [*] Remove quotes from type alias
+ <a href='https://github.com/zulip/zulip/blob/48e8250e2836fc39472518b7bf5d69da543eb768/analytics/views/stats.py#L343'>analytics/views/stats.py:343:16:</a> TC008 [*] Remove quotes from type alias
+ <a href='https://github.com/zulip/zulip/blob/48e8250e2836fc39472518b7bf5d69da543eb768/confirmation/models.py#L76'>confirmation/models.py:76:5:</a> TC008 [*] Remove quotes from type alias
+ <a href='https://github.com/zulip/zulip/blob/48e8250e2836fc39472518b7bf5d69da543eb768/confirmation/models.py#L77'>confirmation/models.py:77:5:</a> TC008 [*] Remove quotes from type alias
+ <a href='https://github.com/zulip/zulip/blob/48e8250e2836fc39472518b7bf5d69da543eb768/zerver/lib/push_notifications.py#L76'>zerver/lib/push_notifications.py:76:49:</a> TC008 [*] Remove quotes from type alias
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TC008 | 9 | 9 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-03-11 17:51_

These all look like false positives except the latchbio cases. 

The Zulip cases require an import that is conditional on some `settings` type other than `TYPE_CHECKING`, so I think unconditionally unquoting those could be a problem.

And the ibis case is a recursive type alias, which I think must be quoted? Possibly the rule doesn't descend into the `Union` because it handles the `... | ...` version fine.

---

_Comment by @Daverball on 2025-03-12 07:47_

> The Zulip cases require an import that is conditional on some `settings` type other than `TYPE_CHECKING`, so I think unconditionally unquoting those could be a problem.

Indeed, it would be a problem, although in order to detect this type of stuff we would need to start tracking all conditional bindings separately, not just `ConditionalDeletion` and I'm not fully convinced it's worth the extra complexity. Although we could probably come up with a more simple heuristic to exclude some bindings, like e.g. if the parent statement is an if/try statement without a corresponding shadowing binding in the other branch, then we reject that binding.

> And the ibis case is a recursive type alias, which I think must be quoted? Possibly the rule doesn't descend into the `Union` because it handles the `... | ...` version fine.

It's because the if branch before it also defines the same symbol, so our naïve approach to bindings assumes the `else` branch has access to the symbol, since it was defined before our type alias definition. If recursive type aliases were broken, the line in the other branch would also yield a false positive.

So this is a similar problem to the other one, although it seems slightly easier to fix, e.g. by walking the parent statements of the the binding and reference in order to determine whether or not they are part of the same if statement, and if they are, whether or not they're part of the same branch. We would also need to do the same thing for match statements.

Red knot might be better suited for detecting these types of things though, since control flow analysis could tell us which bindings are unreachable starting from our reference.

---

_Comment by @MichaReiser on 2025-03-12 08:28_

Let's keep this rule in preview then and create a follow up issue. @dylwil3 has also done some work on control flow analysis in Ruff that might become useful here, once it's ready for prime time.

---

_Comment by @ntBre on 2025-03-12 18:32_

Sounds good, I'll close this for now. 

@Daverball thank you for the additional context here and on some of these other PRs. I really appreciate it!

---

_Closed by @ntBre on 2025-03-12 18:32_

---

_Branch deleted on 2025-05-01 15:32_

---
