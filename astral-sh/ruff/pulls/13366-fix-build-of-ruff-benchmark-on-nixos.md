```yaml
number: 13366
title: "Fix build of `ruff_benchmark` on NixOS"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/fix-jemalloc-conf-nix
created_at: 2024-09-16T07:16:11Z
updated_at: 2024-09-16T07:55:48Z
url: https://github.com/astral-sh/ruff/pull/13366
synced_at: 2026-01-12T15:55:44Z
```

# Fix build of `ruff_benchmark` on NixOS

---

_@MichaReiser_

## Summary


See https://github.com/astral-sh/ruff/pull/13299#issuecomment-2351185007

This PR fixes the `ruff_benchmark` build on NixOS by not relying on the `unprefixed_malloc_on_supported_platforms` feature and instead export the conf as `_rjem_malloc_conf`




---

_Label `internal` added by @MichaReiser on 2024-09-16 07:16_

---

_Comment by @codspeed-hq[bot] on 2024-09-16 07:21_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha/fix-jemalloc-conf-nix)

### Merging #13366 will **improve performances by 4.1%**

<sub>Comparing <code>micha/fix-jemalloc-conf-nix</code> (ae9987e) with <code>main</code> (1365b08)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `micha/fix-jemalloc-conf-nix` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 1.9 ms | 1.8 ms | +4.1% |


---

_Comment by @github-actions[bot] on 2024-09-16 07:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-09-16 07:33_

Hmm okay, now the conf variable isn't picked up anymore. Perfect

---

_Comment by @MichaReiser on 2024-09-16 07:41_

Let's see if this works. I don't see any new decay calls. The perf differences are only re_alloc. 

---

_Merged by @MichaReiser on 2024-09-16 07:41_

---

_Closed by @MichaReiser on 2024-09-16 07:41_

---

_Branch deleted on 2024-09-16 07:41_

---

_Comment by @GaetanLepage on 2024-09-16 07:53_

Backporting this patch to 0.6.5 is working :)

---

_Comment by @MichaReiser on 2024-09-16 07:55_

> Backporting this patch to 0.6.5 is working :)

Nice. Thanks for confirming. Let's hope our benchmarks don't regress to being flaky again.

---
