```yaml
number: 21415
title: "RFC [ruff][ext-lint] 3: external lint interpreter using PyO3"
type: pull_request
state: open
author: pieterh-oai
labels: []
assignees: []
base: main
head: pieterh/ext-lint-3-interpreter
created_at: 2025-11-12T22:14:05Z
updated_at: 2026-01-07T01:49:43Z
url: https://github.com/astral-sh/ruff/pull/21415
synced_at: 2026-01-10T16:30:32Z
```

# RFC [ruff][ext-lint] 3: external lint interpreter using PyO3

---

_Pull request opened by @pieterh-oai on 2025-11-12 22:14_

## Summary

> [!Note]
> This is a stacked PR; the two base commits are PRs #21413 and #21412 . The `[ruff][ext-lint] set up linter runtime` commit is the actual delta for this PR.

This PR brings in the PyO3 dependencies and sets up per-Rayon-thread, per-distinct-config environments to actually run external linters. The build requires `--features ext-lint`; without that it does not include the PyO3 dependency.

The AST that gets ‘projected’ to the individual external linters is still quite minimal, but it gets us an end-to-end example (a G004-like “don’t log eagerly interpolated strings” rule).

There are a bunch of constraints here that restrict what we can do:

- PyO3 doesn’t have support for sub interpreters (or multiprocessing), which would be an easy way to ensure isolation between Rayon threads.
- If we run a single interpreter, we can access it from multiple Rust threads, but parallelism is limited by having to acquire and release the GIL, and we end up serializing the Python execution across worker threads. (In addition, the overhead of locking and unlocking is significant when running short spurts of Python code, which is exactly our workload if we run individual rules on individual AST nodes.) This is so slow as to be somewhat pointless.

The easiest way to get to a working state is to build PyO3 >= 0.23 against a CPython build with freethreading support. In that case, it’s well-defined for multiple Rust threads to call into the same interpreter and execute code, so long as each OS thread is ‘attached.’

The main downside is that we have limited isolation; we do some module name mangling, but a misbehaving rule could modify Python-side global state that would affect other rules.  On the plus side, performance for this implementation is (surprisingly) good.

Python interface: For now, we send over a very barebones node object, and require external rules to implement `check_stmt` or `check_expr` that take a node and a context object as input. The goal was to get to a somewhat-minimal running example; this interface will need more work.

Deployment: this **requires** building against a freethreading CPython (3.13 with `--disable-gil`  or 3.14). To avoid making that a prerequisite for every Ruff build, the `ext-lint` feature is disabled by default. I added some scripts for doing a CPython 3.13 `--disable-gil` build; see the test plan.

## Test Plan

```bash
# manually check Cargo.lock changes
git checkout HEAD^ Cargo.lock
cargo check
# (spot check Cargo.lock for updated version pins)

# test the regular/default build
RUFF_UPDATE_SCHEMA=1 cargo test --workspace --exclude ty
cargo build -p ruff
uvx pre-commit run --all-files --show-diff-on-failure

# if necessary, install and build Python 3.13 --disable-gil
# (from Ruff repo root)
./scripts/nogil/build_python_3_13_nogil.sh ~/cpython_3_13_nogil
source ./scripts/nogil/nogil_env.sh ~/cpython_3_13_nogil

# build and test with feature enabled; then run on some larger repo
cargo build -p ruff --features ext-lint
cargo test --workspace --features ext-lint --exclude ty

cargo build -p ruff --features ext-lint --release
cd large_monorepo
$RUFF_ROOT/target/release/ruff check --select RUFF300 --select-external RLI001 --no-cache .

# ecosystem delta
uvx --from ./python/ruff-ecosystem ruff-ecosystem check "../ruff_baseline/target/debug/ruff" "./target/debug/ruff"
```

---

_Comment by @astral-sh-bot[bot] on 2025-11-12 22:16_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-12 22:17_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, TVDtype@SeriesHE]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5134 diagnostics
+ Found 5133 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2090 diagnostics
+ Found 2091 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@pieterh-oai reviewed on 2025-11-12 22:25_

---

_Review comment by @pieterh-oai on `crates/ruff_linter/src/external/runtime/python.rs`:297 on 2025-11-12 22:25_

This is a holdover from trying to support both gil and non-gil builds. The current state is that only non-GIL really works, so will clean this up.

---

_Comment by @astral-sh-bot[bot] on 2025-11-12 22:26_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -6 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/9968393e4c3a269d6a36c4ec1a8704ce184f0851/superset-extensions-cli/tests/conftest.py#L47'>superset-extensions-cli/tests/conftest.py:47:5:</a> D401 First line of docstring should be in imperative mood: "Default parameters for extension creation."
- <a href='https://github.com/apache/superset/blob/9968393e4c3a269d6a36c4ec1a8704ce184f0851/superset-extensions-cli/tests/test_templates.py#L43'>superset-extensions-cli/tests/test_templates.py:43:5:</a> D401 First line of docstring should be in imperative mood: "Default template context for testing."
- <a href='https://github.com/apache/superset/blob/9968393e4c3a269d6a36c4ec1a8704ce184f0851/superset/mcp_service/mcp_config.py#L160'>superset/mcp_service/mcp_config.py:160:5:</a> D401 First line of docstring should be in imperative mood: "Default MCP auth factory using app.config values."
- <a href='https://github.com/apache/superset/blob/9968393e4c3a269d6a36c4ec1a8704ce184f0851/superset/utils/decorators.py#L215'>superset/utils/decorators.py:215:5:</a> D401 First line of docstring should be in imperative mood: "Default error handler whenever any exception is caught during a SQLAlchemy nested"
- <a href='https://github.com/apache/superset/blob/9968393e4c3a269d6a36c4ec1a8704ce184f0851/superset/utils/machine_auth.py#L62'>superset/utils/machine_auth.py:62:9:</a> D401 First line of docstring should be in imperative mood: "Default AuthDriverFuncType type that sets a session cookie flask-login style"
- <a href='https://github.com/apache/superset/blob/9968393e4c3a269d6a36c4ec1a8704ce184f0851/tests/unit_tests/datasets/conftest.py#L27'>tests/unit_tests/datasets/conftest.py:27:5:</a> D401 First line of docstring should be in imperative mood: "Default props for new columns"
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D401 | 6 | 0 | 6 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -6 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/9968393e4c3a269d6a36c4ec1a8704ce184f0851/superset-extensions-cli/tests/conftest.py#L47'>superset-extensions-cli/tests/conftest.py:47:5:</a> D401 First line of docstring should be in imperative mood: "Default parameters for extension creation."
- <a href='https://github.com/apache/superset/blob/9968393e4c3a269d6a36c4ec1a8704ce184f0851/superset-extensions-cli/tests/test_templates.py#L43'>superset-extensions-cli/tests/test_templates.py:43:5:</a> D401 First line of docstring should be in imperative mood: "Default template context for testing."
- <a href='https://github.com/apache/superset/blob/9968393e4c3a269d6a36c4ec1a8704ce184f0851/superset/mcp_service/mcp_config.py#L160'>superset/mcp_service/mcp_config.py:160:5:</a> D401 First line of docstring should be in imperative mood: "Default MCP auth factory using app.config values."
- <a href='https://github.com/apache/superset/blob/9968393e4c3a269d6a36c4ec1a8704ce184f0851/superset/utils/decorators.py#L215'>superset/utils/decorators.py:215:5:</a> D401 First line of docstring should be in imperative mood: "Default error handler whenever any exception is caught during a SQLAlchemy nested"
- <a href='https://github.com/apache/superset/blob/9968393e4c3a269d6a36c4ec1a8704ce184f0851/superset/utils/machine_auth.py#L62'>superset/utils/machine_auth.py:62:9:</a> D401 First line of docstring should be in imperative mood: "Default AuthDriverFuncType type that sets a session cookie flask-login style"
- <a href='https://github.com/apache/superset/blob/9968393e4c3a269d6a36c4ec1a8704ce184f0851/tests/unit_tests/datasets/conftest.py#L27'>tests/unit_tests/datasets/conftest.py:27:5:</a> D401 First line of docstring should be in imperative mood: "Default props for new columns"
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D401 | 6 | 0 | 6 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Review requested from @amyreese by @amyreese on 2025-11-12 22:43_

---

_Review requested from @MichaReiser by @amyreese on 2025-11-12 22:43_

---

_Review requested from @ntBre by @amyreese on 2025-11-12 22:43_

---

_Review comment by @MichaReiser on `scripts/nogil/build_python_3_13_nogil.sh`:28 on 2025-11-13 08:54_

Can't we use pbs instead of building Python manually?

---

_@MichaReiser reviewed on 2025-11-13 08:54_

---

_Comment by @MichaReiser on 2025-11-13 09:17_

Thank you, this is great. Thanks for taking the time to hack on a prototype. I haven't read the code in detail, so forgive me if any of my feedback or questions should be obvious from the code.

>  On the plus side, performance for this implementation is (surprisingly) good.

Do you have any numbers you can share. How does the performance of this branch compare to:

* Ruff without external linters
* This Ruff version but without any external linters enabled
* This Ruff version with one external linter enabled

My biggest concern (other than performance) are:

* The plugin API, specifically the AST API and how to expose semantic information. We are considering a fundamental rewrite of the AST to make Ruff's parser much faster (and also traversing). Exposing Ruff's AST transparently will likely make this much harder or prevent us from doing this refactor at all. There are also some structural changes that we considered making to our AST that would simplify some handling within Ruff (e.g. make `suite` its own AST node over just a `Vec<Stmt>`). Again, a public API might prevent us from making those changes in the future. Do you have a sense of how expensive it would be to expose a normalized AST (e.g. a Python `ast.parse` compatible AST)? I don't consider this to be strictly blocking. E.g we could version our plugin API but exposing plugins now certainly would increase the upfront cost for any such changes (to a point where they simply become infeasible)
* I believe the current implementation only passes the current AST node without allowing any form of traversal. I think it's essential for plugins to freely traverse the AST (at least downwards)
* The entire: How to integrate plugins into our settings/CLI is a big open question to me. How do we allow plugin-specific options? 
* I think there's some appetite within Astral to compare different plugin systems. E.g. how do Rust-, WASM-, Python-, and Starlark plugin system compare in performance, expressivness, etc. I think we can only answer this after building a set of prototypes.

---

_Comment by @pieterh-oai on 2025-11-13 16:55_

@MichaReiser Thanks for the initial look! I'll keep cleaning this up a little, since it (this PR in particular) is a bit rougher than the 
other two.

## Performance stuff

I can post some flamegraphs that compare running G004 (with `--features ext-lint` and without) and `logging_linter.py` on the same codebase, along with some kinda-anonymized stats. I can also just post up some builds for Mac+Linux so that you can experiment.

## AST and general API stuff

> The plugin API, specifically the AST API and how to expose semantic information.
> [and]
> only passes the current AST node 

I can put up more code to show what this might look like, but it'll take me a few days, most likely. What I have cooking currently is:
* Add additional metadata to `ast.toml` regarding (1) which node types to convert to PyO3+Python classes, and (2) any specific configuration on *how* to do the conversion (e.g. this field should be eagerly populated; this field is populated lazily only through an attribute access).
* Extend `generate.py` to generate the `.pyi` stubs and the PyO3-build-only intermediate types, as well as the Rustland AST -> Pythonland AST. This is tricky but fairly mechanical once it works for ~20 node types.
* Most field accesses can probably be 'lazy', but we will need to do some profiling to see how expensive each call from Python back into Rust actually ends up being.
* Any node type that isn't allowlisted is converted to a RawNode class (which is similar to the current Node in that it just has some basic fields and no ability to traverse parents/children).

...but let me put up the code once it's semi-presentable.
 
+1 on versioning the API. I actually had a note to add that to the TOML (linters specify which version of the API they were built again).

## Integration/Distribution

> The entire: How to integrate plugins into our settings/CLI is a big open question to me. How do we allow plugin-specific options?

I don't think I know enough about the whole ecosystem to be helpful here. At minimum, doing `--all-features` for certain CI steps may become challenging if there are gated builds with different deps.

Aside from that, from my/our point of view, we really want each rule to have some tests, so ideally Ruff would have specific options for that.

## Other languages

> I think there's some appetite within Astral to compare different plugin systems. 

I think that makes sense. We can chat about what are the best options to try. I have code lying around from when I tried RHAI (1 month ago) and RustPython (a few weeks ago).

---

_@pieterh-oai reviewed on 2025-11-13 19:56_

---

_Review comment by @pieterh-oai on `scripts/nogil/build_python_3_13_nogil.sh`:28 on 2025-11-13 19:56_

That'd probably make more sense. Ddoes PBS have configs that include freethreading as a flag already?

---

_Comment by @pieterh-oai on 2025-11-19 20:24_

Rebase; some cleanup:

* Kept `build.rs` but got rid of RUFF_PYTHON_HOME and just rely on PyO3 to find a CPython to use
* Some fixes to nogil_en.sh to fail more cleanly
* Removed alias for `call-callee-regex`

---

_Comment by @pieterh-oai on 2025-11-21 04:11_

Rebase; removed redundant `__init__.py` in the Python-side API. 

---

_Comment by @codspeed-hq[bot] on 2025-11-21 04:25_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
### Merging this PR will **not alter performance**




### Summary

`✅ 53` untouched benchmarks  



---

<sub>Comparing <code>pieterh-oai:pieterh/ext-lint-3-interpreter</code> (41158f8) with <code>main</code> (f97da18)</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/pieterh-oai%3Apieterh%2Fext-lint-3-interpreter?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>



---

_Comment by @MichaReiser on 2025-11-24 09:58_

I think the next step for me on the plugin proposal would be a deep dive on different integration mechanisms and their performance characteristics and how they compare against each other (also in terms of ergonomics).

* Integrating CPython
* Shipping a Python interpreter like rust python
* Using starlark
* Rust plugins

What's the overhead of each of these approaches? Is there an overhead for users not using plugins? What's the cost of enabling one plugin? What's the overhead of running many plugins? What's the most performant way to expose information to plugins? 

---
