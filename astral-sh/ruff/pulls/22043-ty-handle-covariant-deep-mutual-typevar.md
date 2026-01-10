```yaml
number: 22043
title: "[ty] Handle covariant \"deep\" mutual typevar constraints"
type: pull_request
state: open
author: dcreager
labels:
  - ty
assignees: []
draft: true
base: main
head: dcreager/generic-horn-clauses
created_at: 2025-12-18T00:44:47Z
updated_at: 2025-12-19T22:33:00Z
url: https://github.com/astral-sh/ruff/pull/22043
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Handle covariant "deep" mutual typevar constraints

---

_Pull request opened by @dcreager on 2025-12-18 00:44_

This is the first step in supporting https://github.com/astral-sh/ty/issues/2045. This handles just the covariant (i.e. easy) case. With a quick `list` / `Sequence` switcheroo, we get:

```py
def invoke[A, B](fn: Callable[[A], B], value: A) -> B:
    return fn(value)

def head[T](xs: Sequence[T]) -> T: ...
def lift[T](x: T) -> Sequence[T]: ...

reveal_type(invoke(head, [1, 2, 3]))  # revealed: int | Unknown
reveal_type(invoke(lift, 1))  # revealed: Sequence[Literal[1]]
```

---

_Label `ty` added by @dcreager on 2025-12-18 00:44_

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 00:46_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @codspeed-hq[bot] on 2025-12-18 03:01_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fgeneric-horn-clauses?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22043 will **not alter performance**

<sub>Comparing <code>dcreager/generic-horn-clauses</code> (e1f9ba7) with <code>main</code> (cdb7a9f)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  




[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fgeneric-horn-clauses?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @dcreager on 2025-12-19 21:33_

Lovely. The latest mypy_primer timeout happens on `homeassistant/core`. I can reproduce locally. In the `debug` profile, it times out on every run (exit code 124 comes from the `timeout` command):

```
$ ./time.sh
15:58:13 124
15:58:58 124
15:59:43 124
16:00:28 124
16:01:13 124
16:01:58 124
16:02:43 124
16:03:28 124
16:04:13 124
16:04:58 124
```

Under `profiling`, the times out 30-50ish% of the time:

```
$ ./time.sh
15:50:38 1
15:51:08 124
15:51:14 1
15:51:20 1
15:51:51 124
15:51:56 1
15:52:27 124
15:52:57 124
15:53:02 1
15:53:32 124
```

When limited to `TY_MAX_PARALLELISM=1`, it doesn't time out at all.

(Also no timeouts on `main`, so this definitely seems to be a regression introduced by this PR)

---

_Comment by @MichaReiser on 2025-12-19 22:33_

> When limited to TY_MAX_PARALLELISM=1, it doesn't time out at all.

That's scary. It could mean that There's a fixpoint issue (where salsa fails to establish an owner thread). Or does it hang in the sense that cpu usage deops to zero and all threads are blocked?

---
