```yaml
number: 19046
title: Bump CodSpeed to v3
type: pull_request
state: merged
author: adriencaccia
labels:
  - internal
assignees: []
merged: true
base: main
head: chore/bump-codspeed
created_at: 2025-06-30T11:52:15Z
updated_at: 2025-06-30T14:14:58Z
url: https://github.com/astral-sh/ruff/pull/19046
synced_at: 2026-01-12T15:56:30Z
```

# Bump CodSpeed to v3

---

_@adriencaccia_

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
As discussed on Slack, there was an issue with the walltime metrics with `divan`. The issue was fixed with the latest version of `cargo-codspeed` and `codpseed-divan-compat`: https://github.com/CodSpeedHQ/codspeed-rust/releases/tag/v3.0.0.

This PR updates all crates related to CodSpeed. A performance increase of the following benchmarks is expected, as now the correct metric will be used.

```
crates/ruff_benchmark/benches/ty_walltime.rs::multithreaded[pydantic]
crates/ruff_benchmark/benches/ty_walltime.rs::small[altair]
crates/ruff_benchmark/benches/ty_walltime.rs::small[freqtrade]
crates/ruff_benchmark/benches/ty_walltime.rs::small[pydantic]
crates/ruff_benchmark/benches/ty_walltime.rs::small[tanjun]
```

Once this is merged, we will update the historic data of the affected benchmark on the CodSpeed UI, so that no false positives will appear.

---

_Label `internal` added by @AlexWaygood on 2025-06-30 12:01_

---

_Comment by @github-actions[bot] on 2025-06-30 12:01_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
-     memo fields = ~66MB
+     memo fields = ~60MB

rich (https://github.com/Textualize/rich)
- TOTAL MEMORY USAGE: ~129MB
+ TOTAL MEMORY USAGE: ~142MB
-     memo fields = ~106MB
+     memo fields = ~117MB

mkosi (https://github.com/systemd/mkosi)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~129MB
-     memo fields = ~97MB
+     memo fields = ~106MB

vision (https://github.com/pytorch/vision)
-     memo fields = ~304MB
+     memo fields = ~276MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~156MB
+     memo fields = ~171MB

jinja (https://github.com/pallets/jinja)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~106MB

openlibrary (https://github.com/internetarchive/openlibrary)
-     memo fields = ~171MB
+     memo fields = ~189MB

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- TOTAL MEMORY USAGE: ~652MB
+ TOTAL MEMORY USAGE: ~717MB

```
</details>


---

_Comment by @AlexWaygood on 2025-06-30 12:06_

Thanks! It looks like this update is causing our build to fail on Windows? :-(

---

_Comment by @codspeed-hq[bot] on 2025-06-30 12:09_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/adriencaccia%3Achore%2Fbump-codspeed?runnerMode=WallTime)

### Merging #19046 will **improve performances by ×3**

<sub>Comparing <code>adriencaccia:chore/bump-codspeed</code> (8772ea1) with <code>main</code> (db3dcd8)</sub>


#### :tada: Hooray! `codspeed-rust` just leveled up to 3.0.1!

> A heads-up, this is a **breaking change** and it might affect your current performance baseline a bit. But here's the exciting part - it's packed with new, cool features and promises improved result stability :partying_face:!
> _Curious about what's new? Visit our [releases](https://github.com/CodSpeedHQ/codspeed-rust/releases) page to delve into all the awesome details about this new version._

### Summary

`⚡ 5` improvements  
`✅ 3` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` multithreaded[pydantic] `` | 8.7 s | 2.9 s | ×3 |
| ⚡ | `` small[altair] `` | 6.1 s | 3 s | ×2 |
| ⚡ | `` small[freqtrade] `` | 9.2 s | 4.6 s | +99.51% |
| ⚡ | `` small[pydantic] `` | 5.7 s | 2.9 s | +99.42% |
| ⚡ | `` small[tanjun] `` | 4 s | 2 s | +99.73% |


---

_Comment by @github-actions[bot] on 2025-06-30 12:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@ntBre approved on 2025-06-30 12:36_

Thank you! This is good to go as soon as Windows CI is passing.

---

_Review comment by @AlexWaygood on `Cargo.lock`:1 on 2025-06-30 12:37_

I don't think this should block merging since all previous versions of `divan` have been yanked, but would you be able to give some context on why this update adds so many new dependencies to our lockfile? It surprises me a bit given that we already have `default-features = false` for the `codspeed-criterion-compat` dependency :-)

---

_@AlexWaygood reviewed on 2025-06-30 12:37_

---

_@art049 reviewed on 2025-06-30 13:44_

---

_Review comment by @art049 on `Cargo.lock`:1 on 2025-06-30 13:44_

We're adding walltime profiling (with perf) in the integration, so we had to install additional dependencies to communicate with the runner. We'll probably put that behind a feature flag in the future, since we didn't anticipate this could be an issue for you.

---

_@ntBre reviewed on 2025-06-30 13:54_

---

_Review comment by @ntBre on `Cargo.lock`:1 on 2025-06-30 13:54_

I looked at this a bit, and I think the main surprising one for me was `nalgebra`, pulled in by `statrs`, but if y'all need `nalgebra` there's probably not much to do!

---

_Merged by @ntBre on 2025-06-30 14:11_

---

_Closed by @ntBre on 2025-06-30 14:11_

---

_Comment by @ntBre on 2025-06-30 14:11_

Thank you!

---

_Branch deleted on 2025-06-30 14:14_

---
