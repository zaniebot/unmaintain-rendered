```yaml
number: 22004
title: "[ty] Cache legacy generic context"
type: pull_request
state: closed
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
draft: true
base: main
head: micha/cache-legacy-generic-context
created_at: 2025-12-16T11:49:09Z
updated_at: 2025-12-16T12:09:22Z
url: https://github.com/astral-sh/ruff/pull/22004
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Cache legacy generic context

---

_Pull request opened by @MichaReiser on 2025-12-16 11:49_

_No description provided._

---

_Label `internal` added by @MichaReiser on 2025-12-16 11:49_

---

_Label `ty` added by @MichaReiser on 2025-12-16 11:49_

---

_Comment by @astral-sh-bot[bot] on 2025-12-16 11:51_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-16 11:53_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1218:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5124 diagnostics
+ Found 5123 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
-     memo metadata = ~76MB
+     memo metadata = ~80MB


```

</details>




---

_Comment by @codspeed-hq[bot] on 2025-12-16 12:08_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-legacy-generic-context?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22004 will **degrade performances by 4.22%**

<sub>Comparing <code>micha/cache-legacy-generic-context</code> (19881f4) with <code>main</code> (01c0a3e)</sub>



### Summary

`❌ 1` regression  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-legacy-generic-context?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | WallTime | [`` large[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-legacy-generic-context?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 205.4 s | 214.4 s | -4.22% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-legacy-generic-context?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Closed by @MichaReiser on 2025-12-16 12:09_

---
