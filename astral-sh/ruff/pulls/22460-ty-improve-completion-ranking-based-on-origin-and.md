```yaml
number: 22460
title: "[ty] Improve completion ranking based on origin and exact match"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/ranking-improvements
created_at: 2026-01-08T16:07:42Z
updated_at: 2026-01-09T19:14:09Z
url: https://github.com/astral-sh/ruff/pull/22460
synced_at: 2026-01-12T15:57:50Z
```

# [ty] Improve completion ranking based on origin and exact match

---

_@BurntSushi_

This PR does some refactoring to completions and improves ranking based
on the origin of the symbol and whether there is an exact match.

This addresses a lot of the feedback around preferring, e.g., symbols
from `typing` over symbols from other stdlib modules. This also
establishes a more general preference ordering for symbols based on
their origin: current module, project modules, third party modules and
then stdlib modules.

Finally, we give very high ranking to symbols that exactly match what
the end user has typed.

Reviewers are encouraged to read this PR commit-by-commit.

Fixes astral-sh/ty#1274


---

_Review requested from @carljm by @BurntSushi on 2026-01-08 16:07_

---

_Review requested from @MichaReiser by @BurntSushi on 2026-01-08 16:07_

---

_Review requested from @AlexWaygood by @BurntSushi on 2026-01-08 16:07_

---

_Review requested from @Gankra by @BurntSushi on 2026-01-08 16:07_

---

_Review requested from @sharkdp by @BurntSushi on 2026-01-08 16:07_

---

_Review requested from @dcreager by @BurntSushi on 2026-01-08 16:07_

---

_Review request for @dcreager removed by @BurntSushi on 2026-01-08 16:08_

---

_Review request for @carljm removed by @BurntSushi on 2026-01-08 16:08_

---

_Review request for @Gankra removed by @BurntSushi on 2026-01-08 16:08_

---

_Review request for @sharkdp removed by @BurntSushi on 2026-01-08 16:08_

---

_Label `server` added by @BurntSushi on 2026-01-08 16:08_

---

_Label `ty` added by @BurntSushi on 2026-01-08 16:08_

---

_Comment by @BurntSushi on 2026-01-08 16:09_

cc @RasmusNygren in case you want to review here. In particular the refactoring in the earlier commits here.

---

_Comment by @astral-sh-bot[bot] on 2026-01-08 16:09_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-08 16:10_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5363 diagnostics
+ Found 5368 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- Found 1837 diagnostics
+ Found 1838 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5160 diagnostics
+ Found 5161 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:358 on 2026-01-09 08:36_

Nit: You could use `format_compact` here:

https://docs.rs/compact_str/latest/compact_str/macro.format_compact.html

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:285 on 2026-01-09 08:39_

Should we make the fields on `Completion` private and expose getter methods instead to ensure a `Completion` is never modified after computing its relevance?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:117 on 2026-01-09 08:47_

Could `Completion` import `PartialOrd` and `Ord` now that `Relevance` is stored on the `Completion`?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/all_symbols.rs`:70 on 2026-01-09 08:50_

Oops, I think that's on me. I'm not sure how verbose `file` is but we might want to use `file.path(db)` here :)

---

_Review comment by @MichaReiser on `crates/ty_completion_eval/truth/typing-gets-priority/main.py`:19 on 2026-01-09 08:53_

I'm curious how this plays out. E.g. it could be annoying to get a suggestion for a non stdlib `Ordering` just because someone ended up naming their class `Ordering` somewhere in a 2 Million-line project :) But I think it's a good start and we can wait on user reports to see how to improve completions further (e.g. consider the distance between two modules)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:1170 on 2026-01-09 08:57_

Is it intentional that we prioritize third-party dependencies over stdlib?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:1172 on 2026-01-09 08:59_

I guess it's safe to compare by name here but I would probably still use `module.is_known` over matching by name, given that we have this API

---

_@MichaReiser approved on 2026-01-09 09:01_

Nice! 

---

_@BurntSushi reviewed on 2026-01-09 13:21_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:285 on 2026-01-09 13:21_

That's kinda my inclination personally, yeah. Even aside from this new `Relevance` thing being attached to `Completion`.

I'll try this out in a follow-up PR.

---

_@BurntSushi reviewed on 2026-01-09 13:23_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:117 on 2026-01-09 13:23_

It could, but I at least for now chose not to do that. See the `rank` function docs:

```rust
/// Return an ordering relating the two completions.
///
/// A `Ordering::Less` is returned when `c1` should be ranked above
/// `c2`. A `Ordering::Greater` is returned when `c1` should be ranked
/// below `c2`. In other words, a standard ascending sort used with
/// this comparison routine will yields the "best ranked" completions
/// first.
///
/// Note that this could have been implemented via `Eq` and `Ord`
/// impls on `Completion`, but is instead a separate function to avoid
/// conflating relevance ranking with identity.
fn rank(c1: &Completion<'_>, c2: &Completion<'_>) -> Ordering {
    (&c1.relevance, &c1.name).cmp(&(&c2.relevance, &c2.name))
}
```

(I think it's possible we do ultimately do this.)

---

_Review comment by @BurntSushi on `crates/ty_completion_eval/truth/typing-gets-priority/main.py`:19 on 2026-01-09 13:52_

Yeah I'm definitely not 100% sold on this either. Very curious to get user feedback on this.

---

_@BurntSushi reviewed on 2026-01-09 13:52_

---

_@BurntSushi reviewed on 2026-01-09 13:55_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1170 on 2026-01-09 13:55_

That was my thinking yeah. With the idea being that third party dependencies are "more specific" to the user's context.

But this may very well be overruled by the ubiquity and frequency of use for stdlib packages.

Another factor here is that it's _all_ third party code, not just direct dependencies. So it's likelier to be noisier.

---

_@BurntSushi reviewed on 2026-01-09 13:57_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1172 on 2026-01-09 13:57_

Fair enough!

---

_Comment by @astral-sh-bot[bot] on 2026-01-09 14:09_


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

_@MichaReiser reviewed on 2026-01-09 14:14_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:1170 on 2026-01-09 14:14_

Yeah, my thinking here is: if something's available in the standard library, I probably want to use that over any third-party equivalent. 

I'm also surprised that this doesn't regress the `typing` use case, or import `os` as they are now both less specific than a similarly named module in any third-party library.



---

_Comment by @BurntSushi on 2026-01-09 14:20_

I checked out the mypy_primer differences, but they look like flakes. When I re-run it, it still reports differences, but _different_ differences from the previous run. Since there's no conspicuous change in this PR that should lead to such differences, I'm assuming they are flakes.

---

_@BurntSushi reviewed on 2026-01-09 14:43_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1170 on 2026-01-09 14:43_

For `typing`, a lot of the conflicts come from other standard library modules.

The reason why I went this route was this evaluation task:

```
# This test was originally written to
# check that a third party dependency
# gets priority over stdlib. But it was
# not clearly the right choice[1].
#
# 2026-01-09: We stuck with it for now,
# but it seems likely that we'll want
# to regress this task in favor of
# another.
#
# [1]: https://github.com/astral-sh/ruff/pull/22460#discussion_r2676343225
fullma<CURSOR: regex.fullmatch>
```

Where `pyproject.toml` contains the `regex` dependency. In this context, you probably want `fullmatch` from `regex` over the stdlib `re` module. I assume this applies to other third party packages that provide a drop-in replacement for stdlib modules. But maybe this case isn't more important than the ones you point out.

I added the comment above in response here. I think you're ultimately going to be right here, but I'd like to iterate on this in response to user feedback.

---

_Merged by @BurntSushi on 2026-01-09 14:55_

---

_Closed by @BurntSushi on 2026-01-09 14:55_

---

_Branch deleted on 2026-01-09 14:55_

---

_@RasmusNygren reviewed on 2026-01-09 19:14_

LGTM!

---
