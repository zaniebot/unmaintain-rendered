```yaml
number: 18392
title: "[ty] Refactor if narrowing"
type: pull_request
state: closed
author: ericmarkmartin
labels:
  - ty
assignees: []
base: main
head: refactor-if-narrowing
created_at: 2025-05-30T19:29:24Z
updated_at: 2025-06-17T07:56:38Z
url: https://github.com/astral-sh/ruff/pull/18392
synced_at: 2026-01-12T15:56:17Z
```

# [ty] Refactor if narrowing

---

_@ericmarkmartin_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Refactor the semantic index builder’s If statement analysis. This change also lets us detect more unreachable code paths.

## Test Plan

<!-- How was it tested? -->
- Add new tests to unreachable.md
- Update existing tests

---

_Review requested from @carljm by @ericmarkmartin on 2025-05-30 19:29_

---

_Review requested from @AlexWaygood by @ericmarkmartin on 2025-05-30 19:29_

---

_Review requested from @sharkdp by @ericmarkmartin on 2025-05-30 19:29_

---

_Review requested from @dcreager by @ericmarkmartin on 2025-05-30 19:29_

---

_Renamed from "Refactor if narrowing" to "[ty] Refactor if narrowing" by @MichaReiser on 2025-05-30 20:03_

---

_Label `ty` added by @MichaReiser on 2025-05-30 20:03_

---

_Comment by @carljm on 2025-05-30 22:18_

Looks like there are still some failing tests here? Let me know if any of these are things you'd like a second pair of eyes on how to address; otherwise I'll mark this as draft, go ahead and put it back up for review when CI is green.

---

_Converted to draft by @carljm on 2025-05-30 22:18_

---

_Comment by @ericmarkmartin on 2025-05-31 21:40_

> Looks like there are still some failing tests here? Let me know if any of these are things you'd like a second pair of eyes on how to address; otherwise I'll mark this as draft, go ahead and put it back up for review when CI is green.

Looks like we're green now, but mypy primer seems unhappy (sorry if that means I shouldn't have opened it, but I'm not sure how to proceed with primer failures)

---

_Marked ready for review by @ericmarkmartin on 2025-05-31 21:40_

---

_Comment by @sharkdp on 2025-06-04 08:04_

Thank you for working on this!

> Looks like we're green now, but mypy primer seems unhappy

The mypy_primer run was failing due to a network error. I re-ran it now and it fails with
```
fatal[panic] Panicked at crates/ty_python_semantic/src/symbol.rs:927:21 when checking `/tmp/mypy_primer/projects/trio/src/trio/_core/__init__.py`: `internal error: entered unreachable code: If we have at least one declaration, the scope-start should not be definitely visible`
```

This might be related to https://github.com/astral-sh/ty/issues/365#issuecomment-2885927976. I am planning to fix that bug as soon as https://github.com/astral-sh/ruff/pull/18041 is merged (which makes significant changes to the semantic index builder, and has a much higher impact). I suggest that we wait with this PR until these two things have been resolved.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type.md`:130 on 2025-06-06 01:30_

I think the conclusion that this is unreachable code is actually correct -- `type(x)` will never return a `GenericAlias` at runtime, so `type(x) is A[str]` can never be true, only `type(x) is A` can be.

This test probably warrants some expansion, but it doesn't need to happen in this PR. We have an issue open for improving how we handle narrowing against generic types.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/overloads.md`:178 on 2025-06-06 01:30_

We intend to generally silence diagnostics in unreachable code, which should take care of this

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1515 on 2025-06-06 02:05_

As we discussed, it definitely looks weird that we have to record the visibility constraint both before and after. I'm going to take a closer look to try to understand why that is.

---

_@carljm reviewed on 2025-06-06 02:06_

---

_Comment by @carljm on 2025-06-06 02:11_

Not able to reproduce the mypy-primer error running this locally. Will push a rebase and see if it repros in CI.

---

_Comment by @codspeed-hq[bot] on 2025-06-06 02:17_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ericmarkmartin%3Arefactor-if-narrowing)

### Merging #18392 will **degrade performances by 4.14%**

<sub>Comparing <code>ericmarkmartin:refactor-if-narrowing</code> (4af0119) with <code>main</code> (cb8246b)</sub>



### Summary

`❌ 1` regressions  
`✅ 33` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ericmarkmartin%3Arefactor-if-narrowing)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` ty_micro[many_string_assignments] `` | 65.7 ms | 68.6 ms | -4.14% |


---

_Comment by @carljm on 2025-06-06 02:24_

Hmm, well, it definitely repros here. That's odd. This is what I'm running locally:

```
➜ uvx \
            --from="git+https://github.com/hauntsaninja/mypy_primer@01a7ca325f674433c58e02416a867178d1571128" \
            mypy_primer --repo /Users/carlmeyer/projects/ruff \
            --type-checker ty \
            --old main \
            --new refactor-if-narrowing \
            --project-selector "/(trio)\$" \
            --output diff \
            --debug
```

---

_Comment by @carljm on 2025-06-06 02:33_

Ah, figured it out.

The failing file is `src/trio/_core/__init__.py`, which contains this code:

```py
# Windows imports
if sys.platform == "win32":
    from ._run import (
        current_iocp,
        monitor_completion_key,
        readinto_overlapped,
        register_with_iocp,
        wait_overlapped,
        write_overlapped,
    )
# Kqueue imports
elif sys.platform != "linux" and sys.platform != "win32":
    from ._run import current_kqueue, monitor_kevent, wait_kevent
```

And the failure repros locally if I run with `--python-platform="linux"`. Otherwise locally my platform defaults to darwin, and the issue doesn't appear.

Since the repro is an `if`, not a loop with `break`, it seems clear this is an issue introduced by this PR; it's not https://github.com/astral-sh/ty/issues/365#issuecomment-2885927976

---

_Comment by @carljm on 2025-06-10 20:33_

@sharkdp is now working on a larger refactor of reachability and visibility constraints, we should revisit this after that's done and see what is still needed/relevant here. Thanks for the work on this PR, it helped demonstrate the need for this refactor.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-16 22:35_

---

_Comment by @sharkdp on 2025-06-17 07:56_

I'm going to close this, now that #18621 has been merged. @ericmarkmartin thank you for your work here. 

---

_Closed by @sharkdp on 2025-06-17 07:56_

---
