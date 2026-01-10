```yaml
number: 13738
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
created_at: 2024-10-14T01:16:19Z
updated_at: 2024-10-14T07:51:49Z
url: https://github.com/astral-sh/ruff/pull/13738
synced_at: 2026-01-10T20:59:37Z
```

# Update rust-wasm-bindgen monorepo

---

_Pull request opened by @renovate on 2024-10-14 01:16_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [js-sys](https://rustwasm.github.io/wasm-bindgen/) ([source](https://redirect.github.com/rustwasm/wasm-bindgen/tree/HEAD/crates/js-sys)) | workspace.dependencies | patch | `0.3.70` -> `0.3.72` |
| [wasm-bindgen](https://rustwasm.github.io/) ([source](https://redirect.github.com/rustwasm/wasm-bindgen)) | workspace.dependencies | patch | `0.2.93` -> `0.2.95` |
| [wasm-bindgen-test](https://redirect.github.com/rustwasm/wasm-bindgen) | workspace.dependencies | patch | `0.3.43` -> `0.3.45` |

---

### Release Notes

<details>
<summary>rustwasm/wasm-bindgen (wasm-bindgen)</summary>

### [`v0.2.95`](https://redirect.github.com/rustwasm/wasm-bindgen/blob/HEAD/CHANGELOG.md#0295)

[Compare Source](https://redirect.github.com/rustwasm/wasm-bindgen/compare/0.2.94...0.2.95)

Released 2024-10-10

##### Added

-   Added support for implicit discriminants in enums.
    [#&#8203;4152](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4152)

-   Added support for `Self` in complex type expressions in methods.
    [#&#8203;4155](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4155)

##### Changed

-   String enums are no longer generate TypeScript types.
    [#&#8203;4174](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4174)

##### Fixed

-   Fixed generated setters from WebIDL interface attributes binding to wrong JS method names.
    [#&#8203;4170](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4170)

-   Fix string enums showing up in JS documentation and TypeScript bindings without corresponding types.
    [#&#8203;4175](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4175)

***

### [`v0.2.94`](https://redirect.github.com/rustwasm/wasm-bindgen/blob/HEAD/CHANGELOG.md#0294-YANKED)

[Compare Source](https://redirect.github.com/rustwasm/wasm-bindgen/compare/0.2.93...0.2.94)

Released 2024-10-09

##### Added

-   Added support for the WebAssembly `Tail Call` proposal.
    [#&#8203;4111](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4111)

-   Add bindings for `RTCPeerConnection.setConfiguration(RTCConfiguration)` method.
    [#&#8203;4105](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4105)

-   Add bindings to `RTCRtpTransceiverDirection.stopped`.
    [#&#8203;4102](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4102)

-   Added experimental support for `Symbol.dispose` via `WASM_BINDGEN_EXPERIMENTAL_SYMBOL_DISPOSE`.
    [#&#8203;4118](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4118)

-   Added bindings for the draft [WebRTC Encoded Transform](https://www.w3.org/TR/webrtc-encoded-transform) spec.
    [#&#8203;4125](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4125)

-   Added `Debug` implementation to `JsError`.
    [#&#8203;4136](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4136)

-   Added support for `js_name` and `skip_typescript` attributes for string enums.
    [#&#8203;4147](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4147)

-   Added `unsupported` crate to `wasm_bindgen_test(unsupported = test)` as a way of running tests on non-Wasm targets as well.
    [#&#8203;4150](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4150)

-   Added additional bindings for methods taking buffer view types (e.g. `&[u8]`) with corresponding JS types (e.g. `Uint8Array`).
    [#&#8203;4156](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4156)

-   Added additional bindings for setters from WebIDL interface attributes with applicaple parameter types of just `JsValue`.
    [#&#8203;4156](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4156)

##### Changed

-   Implicitly enable reference type and multivalue transformations if the module already makes use of the corresponding target features.
    [#&#8203;4133](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4133)

-   Updated Gamepad API.
    [#&#8203;4134](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4134)

-   Deprecated `Gamepad::display_id` and `GamepadHapticActuator::type_`.
    [#&#8203;4134](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4134)

-   Removed `GamepadAxisMoveEvent`, `GamepadAxisMoveEventInit`, `GamepadButtonEvent`, `GamepadButtonEventInit` and `GamepadServiceTest`, which were seemingly never implemented by any JS environment.
    [#&#8203;4134](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4134)

-   Changed `TextDecoder.decode()` `input` parameter type from `&mut [u8]` to `&[u8]`.
    [#&#8203;4141](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4141)

-   Updated the WebGPU API to the current draft as of 2024-10-07.
    [#&#8203;4145](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4145)

-   Deprecated generated setters from WebIDL interface attribute taking `JsValue` in favor of newer bindings with specific parameter types.
    [#&#8203;4156](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4156)

##### Fixed

-   Fixed linked modules emitting snippet files when not using `--split-linked-modules`.
    [#&#8203;4066](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4066)

-   Fixed incorrect deprecation warning when passing no parameter into `default()` (`init()`) or `initSync()`.
    [#&#8203;4074](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4074)

-   Fixed many proc-macro generated `impl` blocks missing `#[automatically_derived]`, affecting test coverage.
    [#&#8203;4078](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4078)

-   Fixed negative `BigInt` values being incorrectly formatted with two minus signs.
    [#&#8203;4082](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4082)
    [#&#8203;4088](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4088)

-   Fixed emitted `package.json` structure to correctly specify its dependencies
    [#&#8203;4091](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4091)

-   Fixed returning `Option<Enum>` now correctly has the `| undefined` type in TS bindings.
    [#&#8203;4137](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4137)

-   Fixed enum variant name collisions with object prototype fields.
    [#&#8203;4137](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4137)

-   Fixed multiline doc comment alignment and remove empty ones entirely.
    [#&#8203;4135](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4135)

-   Fixed `experimental-nodejs-module` target when used with `#[wasm_bindgen(start)]`.
    [#&#8203;4093](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4093)

-   Fixed error when importing very large JS files.
    [#&#8203;4146](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4146)

-   Specify `"type": "module"` when deploying to nodejs-module
    [#&#8203;4092](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4092)

-   Fixed string enums not generating TypeScript types.
    [#&#8203;4147](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4147)

-   Bindings that take buffer view types (e.g. `&[u8]`) as parameters will now correctly return a `Result` when they might not support a backing `SharedArrayBuffer`. This only applies to new and unstable APIs, which won't cause a breaking in the API.
    [#&#8203;4156](https://redirect.github.com/rustwasm/wasm-bindgen/pull/4156)

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOC4xMTUuMSIsInVwZGF0ZWRJblZlciI6IjM4LjExNS4xIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2024-10-14 01:16_

---

_Comment by @github-actions[bot] on 2024-10-14 01:36_

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

_Comment by @MichaReiser on 2024-10-14 07:31_

I locally tested that the playground still works. 

---

_Merged by @MichaReiser on 2024-10-14 07:38_

---

_Closed by @MichaReiser on 2024-10-14 07:38_

---

_Branch deleted on 2024-10-14 07:38_

---
