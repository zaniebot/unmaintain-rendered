```yaml
number: 18525
title: "Stabilize `starmap-zip` (`RUF058`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: brent/stabilize-ruf058
created_at: 2025-06-07T00:52:27Z
updated_at: 2025-06-10T20:12:00Z
url: https://github.com/astral-sh/ruff/pull/18525
synced_at: 2026-01-12T15:56:20Z
```

# Stabilize `starmap-zip` (`RUF058`)

---

_@ntBre_

## Summary
- Stabilizes RUF058 (starmap-zip) rule by changing it from Preview to Stable
- Migrates test cases from preview_rules to main rules function 
- Updates snapshots accordingly and removes old preview snapshots

## Test plan
- ✅ Migrated tests from preview to main test function
- ✅ `make check` passes
- ✅ `make test` passes  
- ✅ `make citest` passes (no leftover snapshots)

## Rule Documentation
- [Test file](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/mod.rs#L103-L104)
- [Rule documentation](https://docs.astral.sh/ruff/rules/starmap-zip/)

---

_Comment by @github-actions[bot] on 2025-06-09 14:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/e455fab5d3ca2820db2f49c5511a23b168a441ab/libs/core/langchain_core/runnables/router.py#L209'>libs/core/langchain_core/runnables/router.py:209:14:</a> RUF058 [*] `itertools.starmap` called on `zip` iterable
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/b8371f5e6f329bfe1b5f1e099e221c8219fc6bbd/pandas/tests/arithmetic/test_datetime64.py#L2214'>pandas/tests/arithmetic/test_datetime64.py:2214:32:</a> RUF058 [*] `itertools.starmap` called on `zip` iterable
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/27c85f065851b6fc22586ba632e2e754b07fbdc2/astropy/table/soco.py#L81'>astropy/table/soco.py:81:34:</a> RUF058 [*] `itertools.starmap` called on `zip` iterable
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF058 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @ntBre on 2025-06-09 16:32_

---

_@dylwil3 approved on 2025-06-10 12:45_

---

_Renamed from "Stabilize RUF058 (starmap-zip)" to "Stabilize `starmap-zip` (`RUF058`)" by @ntBre on 2025-06-10 14:43_

---

_Merged by @ntBre on 2025-06-10 14:43_

---

_Closed by @ntBre on 2025-06-10 14:43_

---

_Branch deleted on 2025-06-10 14:43_

---

_Added to milestone `v0.12` by @ntBre on 2025-06-10 20:12_

---
