```yaml
number: 19117
title: "[ty] Restructure/move dataclass tests"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/restructure-dataclass-md-tests
created_at: 2025-07-03T10:33:09Z
updated_at: 2025-07-03T10:42:55Z
url: https://github.com/astral-sh/ruff/pull/19117
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Restructure/move dataclass tests

---

_Pull request opened by @sharkdp on 2025-07-03 10:33_

Before I'm adding even more dataclass-related files, let's  organize them in a separate folder.

---

_Review requested from @carljm by @sharkdp on 2025-07-03 10:33_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-03 10:33_

---

_Review requested from @dcreager by @sharkdp on 2025-07-03 10:33_

---

_Label `internal` added by @sharkdp on 2025-07-03 10:33_

---

_Label `testing` added by @sharkdp on 2025-07-03 10:33_

---

_Label `ty` added by @sharkdp on 2025-07-03 10:33_

---

_Merged by @sharkdp on 2025-07-03 10:36_

---

_Closed by @sharkdp on 2025-07-03 10:36_

---

_Branch deleted on 2025-07-03 10:36_

---

_Comment by @github-actions[bot] on 2025-07-03 10:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pegen (https://github.com/we-like-parsers/pegen)
- TOTAL MEMORY USAGE: ~45MB
+ TOTAL MEMORY USAGE: ~41MB

aioredis (https://github.com/aio-libs/aioredis)
-     memo fields = ~60MB
+     memo fields = ~54MB

Expression (https://github.com/cognitedata/Expression)
-     memo fields = ~49MB
+     memo fields = ~54MB

operator (https://github.com/canonical/operator)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~106MB
-     memo fields = ~80MB
+     memo fields = ~88MB

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~80MB

sockeye (https://github.com/awslabs/sockeye)
-     memo fields = ~80MB
+     memo fields = ~72MB

tornado (https://github.com/tornadoweb/tornado)
- TOTAL MEMORY USAGE: ~156MB
+ TOTAL MEMORY USAGE: ~171MB

psycopg (https://github.com/psycopg/psycopg)
- TOTAL MEMORY USAGE: ~228MB
+ TOTAL MEMORY USAGE: ~207MB

django-stubs (https://github.com/typeddjango/django-stubs)
- TOTAL MEMORY USAGE: ~171MB
+ TOTAL MEMORY USAGE: ~189MB

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-07-03 10:42_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Frestructure-dataclass-md-tests?runnerMode=WallTime)

### Merging #19117 will **improve performances by 7.01%**

<sub>Comparing <code>david/restructure-dataclass-md-tests</code> (5e43f35) with <code>main</code> (c4f2eec)</sub>



### Summary

`⚡ 1` improvements  
`✅ 7` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` multithreaded[pydantic] `` | 379.7 ms | 354.8 ms | +7.01% |


---
