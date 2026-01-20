```yaml
number: 22630
title: "[ty] Pluck some low hanging performance fruit for completions"
type: pull_request
state: open
author: BurntSushi
labels:
  - performance
  - server
  - ty
assignees: []
base: main
head: ag/auto-import-perf1
created_at: 2026-01-16T20:09:06Z
updated_at: 2026-01-20T14:25:19Z
url: https://github.com/astral-sh/ruff/pull/22630
synced_at: 2026-01-20T14:40:45Z
```

# [ty] Pluck some low hanging performance fruit for completions

---

_@BurntSushi_

This PR adds a new ad hoc benchmarking CLI for completions and
implements a few low hanging optimizations. All told, for my particular
benchmark (asking for completions on `r` in a blank file within a
checkout of Home Assistant), we get about a 40% improvement (~250ms
down to ~140ms) in the cached case and a more modest ~12.5% improvement
in the uncached case (~600ms down to ~526ms).

Before:

```
$ ./target/profiling/ty_completion_bench ~/astral/relatedclones/scratch-home-assistant/homeassistant/scratch.py 1 -q --iters 30
total elapsed for initial completions request: 603.491595ms
total elapsed: 7.36554807s, time per completion request: 245.518269ms
```

After:

```
$ ./target/profiling/ty_completion_bench ~/astral/relatedclones/scratch-home-assistant/homeassistant/scratch.py 1 -q --iters 30
total elapsed for initial completions request: 526.743638ms
total elapsed: 4.268009725s, time per completion request: 142.26699ms
```

Closes astral-sh/ty#2298


---

_Review requested from @carljm by @BurntSushi on 2026-01-16 20:09_

---

_Review requested from @AlexWaygood by @BurntSushi on 2026-01-16 20:09_

---

_Review requested from @sharkdp by @BurntSushi on 2026-01-16 20:09_

---

_Review requested from @dcreager by @BurntSushi on 2026-01-16 20:09_

---

_Review requested from @MichaReiser by @BurntSushi on 2026-01-16 20:09_

---

_Review requested from @Gankra by @BurntSushi on 2026-01-16 20:09_

---

_Review request for @dcreager removed by @BurntSushi on 2026-01-16 20:09_

---

_Review request for @carljm removed by @BurntSushi on 2026-01-16 20:09_

---

_Review request for @Gankra removed by @BurntSushi on 2026-01-16 20:09_

---

_Review request for @sharkdp removed by @BurntSushi on 2026-01-16 20:09_

---

_Review request for @AlexWaygood removed by @BurntSushi on 2026-01-16 20:09_

---

_Review requested from @Gankra by @BurntSushi on 2026-01-16 20:09_

---

_Label `performance` added by @BurntSushi on 2026-01-16 20:09_

---

_Label `server` added by @BurntSushi on 2026-01-16 20:09_

---

_Label `ty` added by @BurntSushi on 2026-01-16 20:09_

---

_Comment by @astral-sh-bot[bot] on 2026-01-16 20:10_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-16 20:12_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 46 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1825 diagnostics
+ Found 1821 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-16 20:15_


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

_Review comment by @MichaReiser on `crates/ty_completion_bench/Cargo.toml`:1 on 2026-01-17 13:57_

Can you say more about why you chose to create an entirely separate crate over integrating the benchmarks into `ty_walltime` or the Python benchmarks in `scripts/ty_benchmark` (which even includes code to benchmark an LSP server). Integrating into `ty_walltime` has the advantage that we could decide to run the benchmarks as part of our CI pipeline. Integrating it into `ty_benchmark` has the advantage that they're easier to discover. Both benchmark also already provide the necessary infrastructure to install dependency and definitions for common ecosystem projects.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:91 on 2026-01-17 13:57_

Nice! That's a much better approach than my brute force `truncate` call

---

_@MichaReiser reviewed on 2026-01-17 13:59_

---

_@BurntSushi reviewed on 2026-01-20 13:37_

---

_Review comment by @BurntSushi on `crates/ty_completion_bench/Cargo.toml`:1 on 2026-01-20 13:37_

Honestly, the reason is partially just that I wasn't aware of those.  :-) And this particular program is just a copy of `ty_completion_eval` but stripped down for ad hoc benchmarking of completions specifically. So it was extremely quick and easy to add.

Looking at `scripts/ty_benchmark`, it looks like that requires doing a full build of `ty` which means longer iteration times:

```
$ touch ./crates/ty_ide/src/completion.rs && time cargo build --bin ty_completion_bench --profile profiling
   Compiling ty_ide v0.0.0 (/home/andrew/astral/ruff/pr1/crates/ty_ide)
   Compiling ty_completion_bench v0.0.0 (/home/andrew/astral/ruff/pr1/crates/ty_completion_bench)
    Finished `profiling` profile [optimized + debuginfo] target(s) in 3.24s

real    3.313
user    16.869
sys     1.102
maxmem  1227 MB
faults  1767

$ touch ./crates/ty_ide/src/completion.rs && time cargo build --bin ty --profile profiling
   Compiling ty_ide v0.0.0 (/home/andrew/astral/ruff/pr1/crates/ty_ide)
   Compiling ty_server v0.0.0 (/home/andrew/astral/ruff/pr1/crates/ty_server)
   Compiling ty v0.0.0 (/home/andrew/astral/ruff/pr1/crates/ty)
    Finished `profiling` profile [optimized + debuginfo] target(s) in 8.93s

real    9.005
user    1:15.21
sys     1.702
maxmem  1539 MB
faults  2140
```

The program was also made with profiling and ad hoc testing in mind. I'd have to actually go through the process of adding completions to `ty_benchmark` to figure out whether it would work for that. But my program does the minimal amount of work necessary to do a completion request and lets you also repeat that completion request to test the cached case. _And_ you can see the actual completion output, which can be important for understanding what is actually happening. Similarly for `ty_walltime`, which uses divan and doesn't [seem to have a good profiling story](https://github.com/nvzqz/divan/issues/72).

Now to be clear, I _do_ think we should add completion benchmarks to both `ty_benchmark` and `ty_walltime`. Because, yes, running the benchmarks in CI and running them as part of a more complete LSP flow seems like a wise thing to do. But I'd still want this little CLI for ad hoc testing and profiling.

I can add a README noting all of this. Maybe we can get to a point where the CLI isn't needed any more.

---

_Review comment by @BurntSushi on `crates/ty_completion_bench/Cargo.toml`:1 on 2026-01-20 13:59_

I added a README.

---

_@BurntSushi reviewed on 2026-01-20 13:59_

---

_@MichaReiser reviewed on 2026-01-20 13:59_

---

_Review comment by @MichaReiser on `crates/ty_completion_bench/Cargo.toml`:1 on 2026-01-20 13:59_

Thanks for the context. 

I think my concern mainly is this

> Honestly, the reason is partially just that I wasn't aware of those. :-) 

But that's addressed if the plan is to ultimately add those benchmarks to `ty_walltime`, and this is more of an ad-hoc script. 

---

_@MichaReiser approved on 2026-01-20 13:59_

---

_Comment by @BurntSushi on 2026-01-20 14:20_

I created https://github.com/astral-sh/ty/issues/2570 for tracking adding benchmarks to `ty_walltime` at minimum.

---
