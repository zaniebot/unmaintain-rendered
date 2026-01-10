```yaml
number: 19811
title: "[ty] Make `NominalInstanceType` internally wrap an enum"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - performance
  - ty
assignees: []
base: alex/remove-tuples
head: alex/remove-tuples-instance-enum
created_at: 2025-08-07T17:20:14Z
updated_at: 2025-08-12T15:47:07Z
url: https://github.com/astral-sh/ruff/pull/19811
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Make `NominalInstanceType` internally wrap an enum

---

_Pull request opened by @AlexWaygood on 2025-08-07 17:20_

**Stacked on top of https://github.com/astral-sh/ruff/pull/19669; review that PR first!**

---

## Summary

This implements @carljm's suggestion from https://github.com/astral-sh/ruff/pull/19669#issuecomment-3164171196, which appears to buy back almost all the performance and memory regressions from #19669. If we like this approach, I'll merge this PR into that branch shortly before merging that PR. (I'm keeping them as separate PRs for now, for ease of review.)

## Test Plan

All existing tests pass. I don't believe this has any impact on semantics. There is still a small primer report on this PR, but it's now a bit of a mystery to me: I can't reproduce any of the "diagnostics going away" on the target branch of this PR, and I can reproduce all of the "diagnostics being added" on the target branch of this PR. I've seen slightly confusing results from mypy_primer in the past for stacked PRs; I suspect there's a subtle bug in the workflow somewhere that leads to it reporting a diagnostic diff even when there isn't one in cases like this.


---

_Label `ty` added by @AlexWaygood on 2025-08-07 17:20_

---

_Comment by @github-actions[bot] on 2025-08-07 17:23_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-07 17:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     struct metadata = ~3MB
+     struct metadata = ~2MB

trio (https://github.com/python-trio/trio)
-     struct metadata = ~8MB
+     struct metadata = ~7MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     struct fields = ~16MB
+     struct fields = ~15MB

prefect (https://github.com/PrefectHQ/prefect)
-     struct fields = ~35MB
+     struct fields = ~33MB
-     memo metadata = ~84MB
+     memo metadata = ~80MB

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-08-07 17:43_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fremove-tuples-instance-enum?runnerMode=WallTime)

### Merging #19811 will **improve performances by 63.76%**

<sub>Comparing <code>alex/remove-tuples-instance-enum</code> (dd260ae) with <code>alex/remove-tuples</code> (3436d5d)</sub>



### Summary

`⚡ 4` improvements  
`✅ 3` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` large[sympy] `` | 41.3 s | 39.7 s | +4.08% |
| ⚡ | `` medium[colour-science] `` | 10.6 s | 6.5 s | +63.76% |
| ⚡ | `` medium[pandas] `` | 26.4 s | 24.7 s | +6.94% |
| ⚡ | `` small[tanjun] `` | 1.7 s | 1.6 s | +5.65% |


---

_Comment by @codspeed-hq[bot] on 2025-08-07 17:43_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fremove-tuples-instance-enum?runnerMode=Instrumentation)

### Merging #19811 will **improve performances by 6.26%**

<sub>Comparing <code>alex/remove-tuples-instance-enum</code> (dd260ae) with <code>alex/remove-tuples</code> (3436d5d)</sub>



### Summary

`⚡ 1` improvements  
`✅ 41` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` ty_micro[many_string_assignments] `` | 73.9 ms | 69.6 ms | +6.26% |


---

_Comment by @AlexWaygood on 2025-08-08 11:01_

> ```diff
> - tests/test_make.py:866:21: error[no-matching-overload] No overload of function `attrib` matches arguments
> - Found 650 diagnostics
> + Found 649 diagnostics
> ```

I don't understand why this PR would change the outcome here, but I can reproduce it locally... and this seems like an improvement. The code in question looks like this:

```py
import attr
from attr._make import Factory

@attr.s
class C:
    x = attr.ib(factory=list, default=Factory(list))
```

and I think that should be allowed under this overload? https://github.com/python-attrs/attrs/blob/616adf5af936b9e690e72d740440dcfc9df25c87/src/attr/__init__.pyi#L211-L231

At runtime an exception is raised by this `attr.ib()` call:

```pycon
>>> import attr
>>> attr.attrib(default=attr.Factory(list), factory=list)
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    attr.attrib(default=attr.Factory(list), factory=list)
    ~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/alexw/dev/attrs/src/attr/_make.py", line 177, in attrib
    raise ValueError(msg)
ValueError: The `default` and `factory` arguments are mutually exclusive.
```

Mypy catches this and flags it because it has a custom (builtin) plugin for attrs, but pyright's only complaint about that snippet is:

```
/Users/alexw/dev/attrs/foo.py
  /Users/alexw/dev/attrs/foo.py:6:9 - error: Dataclass field without type annotation will cause runtime exception (reportGeneralTypeIssues)
1 error, 0 warnings, 0 informations 
```

which is orthogonal to the issue of whether any of the overloads match.

---

_Comment by @AlexWaygood on 2025-08-08 11:24_

> ```diff
> + scripts/lib/puppet_cache.py:77:10: error[no-matching-overload] No overload of function `NamedTemporaryFile` matches arguments
> ```

but this is definitely a regression (that I can also repro locally). A minimal repro is this:

```py
import tempfile

def f(x: str):
    tempfile.NamedTemporaryFile(prefix=x, suffix=".tar.gz")
```

so it looks like this PR has some unexpected effects on overload solving

---

_Comment by @AlexWaygood on 2025-08-08 13:19_

> > ```diff
> > + scripts/lib/puppet_cache.py:77:10: error[no-matching-overload] No overload of function `NamedTemporaryFile` matches arguments
> > ```
> 
> but this is definitely a regression (that I can also repro locally). A minimal repro is this:
> 
> ```python
> import tempfile
> 
> def f(x: str):
>     tempfile.NamedTemporaryFile(prefix=x, suffix=".tar.gz")
> ```
> 
> so it looks like this PR has some unexpected effects on overload solving

Further investigation reveals that this is a more minimal repro:

```py
def NamedTemporaryFile[T: (str, bytes)](suffix: T | None, prefix: T | None) -> None: ...


def f(x: str):
    NamedTemporaryFile(prefix=x, suffix=".tar.gz")
```

The error we emit on this is:

```
error[invalid-argument-type]: Argument to function `NamedTemporaryFile` is incorrect
 --> bar.py:5:24
  |
4 | def f(x: str):
5 |     NamedTemporaryFile(prefix=x, suffix=".tar.gz")
  |                        ^^^^^^^^ Expected `Literal[".tar.gz"] | None`, found `str`
  |
```

So it's nothing to do with overload solving; it's to do with TypeVar solving.

---

_Comment by @AlexWaygood on 2025-08-08 13:26_

Hah, and the bug is in the commit that was nearly all written by Claude... that means it's not my fault, right? ;)

---

_Marked ready for review by @AlexWaygood on 2025-08-08 16:57_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-08 16:57_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-08 16:57_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-08 16:57_

---

_Label `performance` added by @AlexWaygood on 2025-08-08 16:57_

---

_Comment by @AlexWaygood on 2025-08-08 17:47_

Well... there's some bad news here. I just noticed that mypy_primer runs seem to be taking a very long time on this PR branch. It looks like it takes 10 minutes to type-check static-frame with this PR branch in mypy_primer, whereas it takes around 1 minute to type-check static-frame with https://github.com/astral-sh/ruff/pull/19669. I can reproduce a severe slowdown locally on this branch for that repo too. No idea why that would be yet.

---

_Comment by @AlexWaygood on 2025-08-08 19:08_

Okay, https://github.com/astral-sh/ruff/pull/19811/commits/fbcb8e344dac2c2065380e94b7a70f6c04ebdaae fixes the `static-frame` perf regression for me locally! It might be worthwhile for us to add `static-frame` as a benchmark -- I can look into that as a followup.

Edit: and it fixed it in CI as well -- mypy_primer is back to completing in ~3m!

---

_Comment by @AlexWaygood on 2025-08-08 22:22_

> Okay, [fbcb8e3](https://github.com/astral-sh/ruff/commit/fbcb8e344dac2c2065380e94b7a70f6c04ebdaae) fixes the `static-frame` perf regression for me locally! It might be worthwhile for us to add `static-frame` as a benchmark -- I can look into that as a followup.
> 
> Edit: and it fixed it in CI as well -- mypy_primer is back to completing in ~3m!

And in fact, I'm splitting that out as a standalone change, since it improves performance relative to `main` as well: https://github.com/astral-sh/ruff/pull/19840

---

_Closed by @AlexWaygood on 2025-08-08 22:48_

---

_Reopened by @AlexWaygood on 2025-08-08 22:48_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:225 on 2025-08-10 10:03_

See https://github.com/astral-sh/ruff/pull/19669#discussion_r2264092114

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:119 on 2025-08-10 10:07_

I moved this method from `class.rs` to `instance.rs` so that we could avoid having to materialize the `ClassType` if the instance type was an `ExactTuple` variant internally.

Now that I've added caching to `TupleType::to_class_type()`, this might not be a particularly significant optimisation anymore. But with our current callsites across the repo, we only ever need to know the tuple spec of instances, never the tuple spec of `Type::GenericAlias`es. So there doesn't appear to be a huge amount of downside to making this a method on `NominalInstanceType` rather than a method on `ClassType`.

`slice_literal` could also possibly be made a method on `NominalInstanceType` rather than `ClassType`, but it doesn't seem to have any performance benefits from what I can tell locally, and it would make this PR's diff bigger.

---

_@AlexWaygood reviewed on 2025-08-10 10:17_

---

_@AlexWaygood reviewed on 2025-08-10 15:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:229 on 2025-08-10 15:28_

This `.expect()` call means we now panic if we're given a custom typeshed without a `tuple` class. I don't _love_ this but I also think it's _okay_ -- there are already classes where we panic if they're not available in typeshed, such as `object` and `types.ModuleType`. `tuple` is similarly special; I think it's okay if we effectively mandate that custom typesheds have to have a `tuple` class in them.

Doing this refactor without the `.expect()` call here would be quite a bit harder, I think.

---

_Comment by @AlexWaygood on 2025-08-11 10:52_

I've merged the changes here directly into #19669

---

_Closed by @AlexWaygood on 2025-08-11 10:52_

---

_Branch deleted on 2025-08-11 10:52_

---
