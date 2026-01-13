```yaml
number: 17448
title: "Use keyring authentication when retrieving `tool@latest` version"
type: pull_request
state: open
author: zanieb
labels:
  - bug
assignees: []
base: main
head: claude/debug-issue-17436-XRwg1
created_at: 2026-01-13T22:06:09Z
updated_at: 2026-01-13T22:20:03Z
url: https://github.com/astral-sh/uv/pull/17448
synced_at: 2026-01-13T22:36:21Z
```

# Use keyring authentication when retrieving `tool@latest` version

---

_@zanieb_

Closes #17436


---

_Label `bug` added by @zanieb on 2026-01-13 22:10_

---

_Comment by @codspeed-hq[bot] on 2026-01-13 22:20_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/zaniebot%3Aclaude%2Fdebug-issue-17436-XRwg1?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **degrade performance by 10.41%**

<sub>Comparing <code>zaniebot:claude/debug-issue-17436-XRwg1</code> (a492619) with <code>main</code> (c6a0287)</sub>



### Summary

`❌ 1` regressed benchmark  
`✅ 1` untouched benchmark  
`⏩ 3` skipped benchmarks[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/zaniebot%3Aclaude%2Fdebug-issue-17436-XRwg1?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | Simulation | [`` resolve_warm_jupyter ``](https://codspeed.io/astral-sh/uv/branches/zaniebot%3Aclaude%2Fdebug-issue-17436-XRwg1?uri=crates%2Fuv-bench%2Fbenches%2Fuv.rs%3A%3Auv%3A%3Aresolve_warm_jupyter%3A%3Aresolve_warm_jupyter&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 71.9 ms | 80.3 ms | -10.41% |

[^skipped]: 3 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/uv/branches/zaniebot%3Aclaude%2Fdebug-issue-17436-XRwg1?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---
