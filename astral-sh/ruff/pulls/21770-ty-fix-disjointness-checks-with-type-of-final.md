```yaml
number: 21770
title: "[ty] Fix disjointness checks with type-of `@final` classes"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
assignees: []
merged: true
base: main
head: ibraheem/type-of-final-class-disjointness
created_at: 2025-12-03T06:19:48Z
updated_at: 2025-12-10T20:15:13Z
url: https://github.com/astral-sh/ruff/pull/21770
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Fix disjointness checks with type-of `@final` classes

---

_Pull request opened by @ibraheemdev on 2025-12-03 06:19_

## Summary

We currently perform a subtyping check, similar to what we were doing for `@final` instances before https://github.com/astral-sh/ruff/pull/21167, which is incorrect, e.g. we currently consider `type[X[Any]]` and `type[X[T]]]` disjoint (where `X` is `@final`).

Stacked on https://github.com/astral-sh/ruff/pull/21769.

---

_Label `ty` added by @ibraheemdev on 2025-12-03 06:19_

---

_Comment by @astral-sh-bot[bot] on 2025-12-03 06:21_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-03 06:25_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 491 diagnostics
+ Found 489 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/display.py:483:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/display.py:486:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/display.py:517:85: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/frame.py:5335:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/frame.py:5346:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/frame.py:7157:74: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/index.py:251:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/index.py:689:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/index.py:1117:80: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/index_hierarchy.py:556:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/index_hierarchy.py:557:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/index_hierarchy.py:558:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/pivot.py:248:76: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/pivot.py:249:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/pivot.py:250:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/pivot.py:252:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/pivot.py:253:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/pivot.py:260:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/reduce.py:462:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/reduce.py:463:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/reduce.py:486:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/reduce.py:487:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/series.py:2106:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/type_blocks.py:1881:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/type_blocks.py:1883:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/util.py:444:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/util.py:445:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/util.py:446:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/util.py:448:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/util.py:449:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/util.py:452:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/util.py:453:89: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/util.py:455:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/util.py:457:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/util.py:459:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/util.py:2094:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/util.py:2095:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/util.py:2096:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1833 diagnostics
+ Found 1795 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5259 diagnostics
+ Found 5258 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @codspeed-hq[bot] on 2025-12-03 06:41_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Ftype-of-final-class-disjointness?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21770 will **not alter performance**

<sub>Comparing <code>ibraheem/type-of-final-class-disjointness</code> (e4e1ff2) with <code>main</code> (5dc0079)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Ftype-of-final-class-disjointness?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_@ibraheemdev reviewed on 2025-12-03 11:27_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/type_of/basic.md`:257 on 2025-12-03 11:27_

There are a couple false negatives here, including this one (see https://github.com/astral-sh/ruff/pull/21769#discussion_r2584496310).

---

_Marked ready for review by @ibraheemdev on 2025-12-03 11:28_

---

_Review requested from @carljm by @ibraheemdev on 2025-12-03 11:28_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-12-03 11:28_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-12-03 11:28_

---

_Review requested from @dcreager by @ibraheemdev on 2025-12-03 11:28_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_of/basic.md`:276 on 2025-12-04 00:40_

This comment seems too weak, at least if it's meant to be apply to the next two lines specifically? It doesn't matter what `T` and `U` specialize to; `BivSub` is bivariant in `T`, so `BivSub[T]` and `BivSub[U]` are equivalent, and thus so are `type[BivSub[T]]` and `type[BivSub[U]]`.

But maybe it's meant to apply in general to all the tests in this function, in which case it makes sense for the non-bivariant cases; maybe should just be moved down.

---

_@carljm approved on 2025-12-09 23:37_

---

_Comment by @sharkdp on 2025-12-10 19:37_

Oh, it looks like you're solving some some tests with TODOs that I added earlier today (https://github.com/astral-sh/ty/issues/1842)

---

_Comment by @ibraheemdev on 2025-12-10 19:52_

@sharkdp I think I've fixed those by delegating to the default specialization, now that we have an implementation for disjointness between generic aliases.

---

_Merged by @ibraheemdev on 2025-12-10 20:15_

---

_Closed by @ibraheemdev on 2025-12-10 20:15_

---

_Branch deleted on 2025-12-10 20:15_

---
