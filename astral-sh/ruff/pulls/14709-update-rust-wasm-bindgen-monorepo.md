```yaml
number: 14709
title: Update rust-wasm-bindgen monorepo
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/rust-wasm-bindgen-monorepo
created_at: 2024-12-02T00:47:58Z
updated_at: 2024-12-02T01:03:45Z
url: https://github.com/astral-sh/ruff/pull/14709
synced_at: 2026-01-12T15:55:48Z
```

# Update rust-wasm-bindgen monorepo

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [js-sys](https://rustwasm.github.io/wasm-bindgen/) ([source](https://redirect.github.com/rustwasm/wasm-bindgen/tree/HEAD/crates/js-sys)) | workspace.dependencies | patch | `0.3.72` -> `0.3.74` |
| [wasm-bindgen](https://rustwasm.github.io/) ([source](https://redirect.github.com/rustwasm/wasm-bindgen)) | workspace.dependencies | patch | `0.2.95` -> `0.2.97` |
| [wasm-bindgen-test](https://redirect.github.com/rustwasm/wasm-bindgen) | workspace.dependencies | patch | `0.3.45` -> `0.3.47` |

---

### Release Notes

<details>
<summary>rustwasm/wasm-bindgen (wasm-bindgen)</summary>

### [`v0.2.97`](https://redirect.github.com/rustwasm/wasm-bindgen/blob/HEAD/CHANGELOG.md#0297)

[Compare Source](https://redirect.github.com/rustwasm/wasm-bindgen/compare/0.2.96...0.2.97)

Released 2024-11-30

##### Fixed

-   Fixed `js-sys` and `wasm-bindgen-futures` relying on internal paths of `wasm-bindgen` that are not crate feature additive.
    [#&#8203;4305](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4305)

***

### [`v0.2.96`](https://redirect.github.com/rustwasm/wasm-bindgen/blob/HEAD/CHANGELOG.md#0296)

[Compare Source](https://redirect.github.com/rustwasm/wasm-bindgen/compare/0.2.95...0.2.96)

Released 2024-11-29

##### Added

-   Added support for the [`HTMLOrSVGElement`](https://html.spec.whatwg.org/#htmlorsvgelement) `mixin`, which is used for all interfaces deriving from `Element`.
    [#&#8203;4143](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4143)

-   Added bindings for [MathMLElement](https://www.w3.org/TR/MathML3).
    [#&#8203;4143](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4143)

-   Added JSDoc type annotations to C-style enums.
    [#&#8203;4192](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4192)

-   Added support for C-style enums with negative discriminants.
    [#&#8203;4204](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4204)

-   Added bindings for `MediaStreamTrack.getCapabilities`.
    [#&#8203;4236](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4236)

-   Added WASM ABI support for `u128` and `i128`
    [#&#8203;4222](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4222)

-   Added support for the `wasm32v1-none` target.
    [#&#8203;4277](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4277)

-   Added support for `no_std` to `js-sys`, `web-sys`, `wasm-bindgen-futures` and `wasm-bindgen-test`.
    [#&#8203;4277](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4277)

-   Added support for `no_std` to `link_to!`, `static_string` (via `thread_local_v2`) and `throw`.
    [#&#8203;4277](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4277)

-   Added environment variables to configure tests: `WASM_BINDGEN_USE_BROWSER`, `WASM_BINDGEN_USE_DEDICATED_WORKER`, `WASM_BINDGEN_USE_SHARED_WORKER` `WASM_BINDGEN_USE_SERVICE_WORKER`, `WASM_BINDGEN_USE_DENO` and `WASM_BINDGEN_USE_NODE_EXPERIMENTAL`. The use of `wasm_bindgen_test_configure!` will overwrite any environment variable.
    [#&#8203;4295](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4295)

##### Changed

-   String enums now generate private TypeScript types but only if used.
    [#&#8203;4174](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4174)

-   Remove unnecessary JSDoc type annotations from generated `.d.ts` files
    [#&#8203;4187](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4187)

-   Deprecate `autofocus`, `tabIndex`, `focus()` and `blur()` bindings in favor of bindings on the inherited `Element` class.
    [#&#8203;4143](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4143)

-   Optimized ABI performance for `Option<{i32,u32,isize,usize,f32,*const T,*mut T}>`.
    [#&#8203;4183](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4183)

-   Deprecate `--reference-types` in favor of automatic target feature detection.
    [#&#8203;4237](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4237)

-   `wasm-bindgen-test-runner` now tries to restart the WebDriver on failure, instead of spending its timeout period trying to connect to a non-existing WebDriver.
    [#&#8203;4267](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4267)

-   Deprecated `#[wasm_bindgen(thread_local)]` in favor of `#[wasm_bindgen(thread_local_v2)]`, which creates a `wasm_bindgen::JsThreadLocal`. It is similar to `std::thread::LocalKey` but supports `no_std`.
    [#&#8203;4277](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4277)

-   Updated the WebGPU API to the current draft as of 2024-11-22.
    [#&#8203;4290](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4290)

-   Improved error messages for `self` arguments in invalid positions.
    [#&#8203;4276](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4276)

##### Fixed

-   Fixed methods with `self: &Self` consuming the object.
    [#&#8203;4178](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4178)

-   Fixed unused string enums generating JS values.
    [#&#8203;4193](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4193)

-   Fixed triggering lints in testing facilities.
    [#&#8203;4195](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4195)

-   Fixed `#[should_panic]` not working with `#[wasm_bindgen_test(unsupported = ...)]`.
    [#&#8203;4196](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4196)

-   Fixed potential `null` error when using `JsValue::as_debug_string()`.
    [#&#8203;4192](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4192)

-   Fixed generated types when the getter and setter of a property have different types.
    [#&#8203;4202](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4202)

-   Fixed generated types when a static getter/setter has the same name as an instance getter/setter.
    [#&#8203;4202](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4202)

-   Fixed invalid TypeScript return types for multivalue signatures.
    [#&#8203;4210](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4210)

-   Only emit `table.fill` instructions if the bulk-memory proposal is enabled.
    [#&#8203;4237](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4237)

-   Fixed calls to `JsCast::instanceof()` not respecting JavaScript namespaces.
    [#&#8203;4241](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4241)

-   Fixed imports for functions using `this` and late binding.
    [#&#8203;4225](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4225)

-   Don't expose non-functioning implicit constructors to classes when none are provided.
    [#&#8203;4282](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4282)

***

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

👻 **Immortal**: This PR will be recreated if closed unmerged. Get [config help](https://redirect.github.com/renovatebot/renovate/discussions) if that's undesired.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xOS4wIiwidXBkYXRlZEluVmVyIjoiMzkuMTkuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2024-12-02 00:47_

---

_Comment by @github-actions[bot] on 2024-12-02 01:03_

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

_Merged by @charliermarsh on 2024-12-02 01:03_

---

_Closed by @charliermarsh on 2024-12-02 01:03_

---

_Branch deleted on 2024-12-02 01:03_

---
