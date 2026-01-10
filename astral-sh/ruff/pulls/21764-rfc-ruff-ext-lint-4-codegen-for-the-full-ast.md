```yaml
number: 21764
title: "RFC [ruff][ext-lint] 4: codegen for the full AST"
type: pull_request
state: open
author: pieterh-oai
labels: []
assignees: []
base: main
head: pieterh/ext-lint-4-ast-codegen
created_at: 2025-12-02T20:06:28Z
updated_at: 2026-01-07T01:50:42Z
url: https://github.com/astral-sh/ruff/pull/21764
synced_at: 2026-01-10T16:30:32Z
```

# RFC [ruff][ext-lint] 4: codegen for the full AST

---

_Pull request opened by @pieterh-oai on 2025-12-02 20:06_

## Summary

This PR extends `generate.py` to codegen Python shims for the (entire) Ruff AST,
replacing catchall Node in the previous PR (#21415). The shims work as follows:

* Python AST node classes (and objects) are created entirely on the Rust side
  as PyO3 types.
* From the Python code's point of view, node objects have the expected fields
  with names and types that line up with the Rust AST. The 'API' is a set of `pyi`
  files in `ruff_linter/resources/`.
* The majority of AST node fields are lazily loaded (from the Rust AST to the
  projected Python AST) on first use. A subset of fields are marked as 'eager'
  (e.g. `op` for `AugAssign` is string rather than a node subclass). The
  configuration of lazy vs. eager projection is configured in `ast.toml`.
* We use `RawNode` as a catch-all for any node types that are somehow not
  covered by the projection code. This means it's effectively unused right
  now, but might be in the future if AST changes cause the codegen to be out
  of date.

The codegen in `generate.py` needs to generate 3 things:
1. PyO3 types for each AST node type; see `write_projection_bindings` and
  the result in `ruff_linter/src/external/ast/python/generated.rs`.
2. Per-node-type projection code; see `write_projection_helpers` and
  the corresponding output in `projection.rs`
3. Python `.pyi` interface files; see `write_python_stub` and
  the output in `ruff_linter/resources/ruff_external`.

## Lifetime-related considerations

The challenging parts (at least for me) were handling lifetime differences
between PyO3 AST node objects and the Ruff types that they need to refer to,
the `SourceFile` and the corresponding Ruff AST node.

Once on the Python-side heap,  it's not really possible to limit object lifetime
(since we can't really make assertions when their refcount will hit 0, or
if they'll need to be GC'd).  This means that we can't include direct references to the
shorter-lived Rust-side objects in any PyO3 objects.

It's not too hard to satisfy the Ruff compiler by cloning Ruff AST nodes and `Arc`
references to the source, but that isn't efficient. Copying entire AST subtrees
defeats the purpose of making field access lazy in the first place, and holding
references through the `SourceFile` would mean that any file with at least 1 Python
rule execution (even if it yields 0 results) will be held in memory until end of process
(or, at best, a future CPython GC cycle, I guess?).

On the flip side, if we avoid copying nodes and strong references to the source,
we need to lifecycle-manage this so that the Python code fails in a predictable
way. We probably want to fail early if a Python rule does something it shouldn't
(like storing nodes in a global and then trying to use that across `check_expr`
invocations), rather than arbitrarily failing later.

This is what I ended up doing:

* `AstStore` maps ID numbers to Ruff AST nodes to support lazy loading and
  (in a future PR) Semantic calls. This mapping is short-lived (per rule
  invocation), and the `ExternalCheckerContext` explicitly calls `invalidate`
  so that, as soon as rule dispatch ends, future access to Python AST nodes
  will fail (* at least for any fields that were not already loaded).

* `SourceFileHandle` uses a threadlocal global to store the current `SourceFile`;
  once it goes out of scope, the reference to `SourceFile` is unset and any
  subsequent calls to `locator` will fail.

The code in `store.rs` and `source.rs` does more or less the same thing; I'd
be interested in feedback on how to clean this up more / how to make this
more idiomatic.

## Other tradeoffs:

* Lazy loading means that we generate very few Python heap objects up front.
  Calls from Python into Rust appear to be relatively inexpensive because
  of the 'single interpreter with multiple attached OS threads that
  don't interact' setup.

* This exposes the Ruff AST directly (though the codegen is structured
  in a way that could permit some rewriting).
   * The downside is that future changes to the AST will directly affect the
     Python API.
   * The advantage is that we can expose APIs like `Semantic` directly
     to Python rules. This means that Python rules look/feel very
     similar to equivalent rules implemented in Rust.

* This is obviously a big PR, with 1kloc of new stuff in `generate.py`
  and 10kloc of generated code. I have put some, but not a lot, of work
  into trying to simplify the codegen parts.

## Test Plan

```
cargo test --workspace --exclude ty
source ./scripts/nogil/nogil_env.sh ~/cpython_3_13_nogil/
cargo test --workspace --exclude ty --features ext-lint
uvx pre-commit run --all-files --show-diff-on-failure
```

---

_Renamed from "[ruff][ext-lint] 4: codegen for the full AST" to "RFC [ruff][ext-lint] 4: codegen for the full AST" by @pieterh-oai on 2025-12-02 20:06_

---

_Comment by @astral-sh-bot[bot] on 2026-01-07 01:42_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-07 01:43_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

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
- Found 5528 diagnostics
+ Found 5533 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- Found 1837 diagnostics
+ Found 1836 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5134 diagnostics
+ Found 5131 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14473 diagnostics
+ Found 14474 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-07 01:50_


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
