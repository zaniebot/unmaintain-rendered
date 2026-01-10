```yaml
number: 12975
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
created_at: 2024-08-19T00:23:00Z
updated_at: 2024-08-19T00:48:30Z
url: https://github.com/astral-sh/ruff/pull/12975
synced_at: 2026-01-10T21:38:32Z
```

# Update rust-wasm-bindgen monorepo

---

_Pull request opened by @renovate on 2024-08-19 00:23_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [js-sys](https://rustwasm.github.io/wasm-bindgen/) ([source](https://togithub.com/rustwasm/wasm-bindgen/tree/HEAD/crates/js-sys)) | workspace.dependencies | patch | `0.3.69` -> `0.3.70` |
| [wasm-bindgen](https://rustwasm.github.io/) ([source](https://togithub.com/rustwasm/wasm-bindgen)) | workspace.dependencies | patch | `0.2.92` -> `0.2.93` |
| [wasm-bindgen-test](https://togithub.com/rustwasm/wasm-bindgen) | workspace.dependencies | patch | `0.3.42` -> `0.3.43` |

---

### Release Notes

<details>
<summary>rustwasm/wasm-bindgen (wasm-bindgen)</summary>

### [`v0.2.93`](https://togithub.com/rustwasm/wasm-bindgen/blob/HEAD/CHANGELOG.md#0293)

[Compare Source](https://togithub.com/rustwasm/wasm-bindgen/compare/0.2.92...0.2.93)

Released 2024-08-13

##### Added

-   Allow exporting functions named `default`. Throw error in wasm-bindgen-cli if --target web and
    an exported symbol is named `default`.
    [#&#8203;3930](https://togithub.com/rustwasm/wasm-bindgen/pull/3930)

-   Added support for arbitrary expressions when using `#[wasm_bindgen(typescript_custom_section)]`.
    [#&#8203;3901](https://togithub.com/rustwasm/wasm-bindgen/pull/3901)

-   Implement `From<NonNull<T>>` for `JsValue`.
    [#&#8203;3877](https://togithub.com/rustwasm/wasm-bindgen/pull/3877)

-   Add method `copy_within` for TypedArray, add methods `find_last`,`find_last_index` for Array.
    [#&#8203;3888](https://togithub.com/rustwasm/wasm-bindgen/pull/3888)

-   Added support for returning `Vec`s from async functions.
    [#&#8203;3630](https://togithub.com/rustwasm/wasm-bindgen/pull/3630)

-   Added bindings for `InputDeviceInfo` and `MediaTrackCapabilities`.
    [#&#8203;3935](https://togithub.com/rustwasm/wasm-bindgen/pull/3935)

-   Add bindings for `RTCRtpReceiver.getCapabilities(DOMString)` method.
    [#&#8203;3941](https://togithub.com/rustwasm/wasm-bindgen/pull/3941)

-   Add bindings for `VisualViewport`.
    [#&#8203;3931](https://togithub.com/rustwasm/wasm-bindgen/pull/3931)

-   Add bindings for `queueMicrotask`.
    [#&#8203;3981](https://togithub.com/rustwasm/wasm-bindgen/pull/3981)

-   Add experimental bindings for User Agent Client Hints API
    [#&#8203;3989](https://togithub.com/rustwasm/wasm-bindgen/pull/3989)

-   Add bindings for `FocusOptions`.
    [#&#8203;3996](https://togithub.com/rustwasm/wasm-bindgen/pull/3996)

-   Add bindings for `RTCRtpReceiver.jitterBufferTarget`.
    [#&#8203;3968](https://togithub.com/rustwasm/wasm-bindgen/pull/3968)

-   Generate getters for all WebIDL dictionary types.
    [#&#8203;3993](https://togithub.com/rustwasm/wasm-bindgen/pull/3993)

-   Support for iterable in WebIDL. Gives `entries`, `keys`, `values` methods for regular and asynchronous, as well as `for_each` for regular, iterables.
    [#&#8203;3962](https://togithub.com/rustwasm/wasm-bindgen/pull/3962)

-   Add bindings for `HTMLTableCellElement.abbr` and `scope` properties.
    [#&#8203;3972](https://togithub.com/rustwasm/wasm-bindgen/pull/3972)

-   Add WebIDL definitions relating to `Popover API`.
    [#&#8203;3977](https://togithub.com/rustwasm/wasm-bindgen/pull/3977)

-   Added the `thread_stack_size` property to the object parameter of `default()` (`init()`) and `initSync()`, making it possible to set the stack size of spawned threads. `__wbindgen_thread_destroy()` now has a third optional parameter for the stack size, the default stack size is assumed when not passing it. When calling from the thread to be destroyed, by passing no parameters, the correct stack size is determined internally.
    [#&#8203;3995](https://togithub.com/rustwasm/wasm-bindgen/pull/3995)

-   Added bindings to the Device Memory API.
    [#&#8203;4011](https://togithub.com/rustwasm/wasm-bindgen/pull/4011)

-   Added support for WebIDL records. This added new methods to various APIs, notably `ClipboardItem()`, `GPUDeviceDescriptor.requiredLimits` and `Header()`.
    [#&#8203;4030](https://togithub.com/rustwasm/wasm-bindgen/pull/4030)

-   Added an official MSRV policy. Library MSRV changes will be accompanied by a minor version bump. CLI tool MSRV can change with any version bump.
    [#&#8203;4038](https://togithub.com/rustwasm/wasm-bindgen/pull/4038)

-   Added bindings to `NavigatorOptions.vibrate`.
    [#&#8203;4041](https://togithub.com/rustwasm/wasm-bindgen/pull/4041)

-   Added an experimental Node.JS ES module target, in comparison the current `node` target uses CommonJS, with `--target experimental-nodejs-module` or when testing with `wasm_bindgen_test_configure!(run_in_node_experimental)`.
    [#&#8203;4027](https://togithub.com/rustwasm/wasm-bindgen/pull/4027)

-   Added importing strings as `JsString` through `#[wasm_bindgen(thread_local, static_string)] static STRING: JsString = "a string literal";`.
    [#&#8203;4055](https://togithub.com/rustwasm/wasm-bindgen/pull/4055)

-   Added experimental test coverage support for `wasm-bindgen-test-runner`, see the guide for more information.
    [#&#8203;4060](https://togithub.com/rustwasm/wasm-bindgen/pull/4060)

##### Changed

-   Stabilize Web Share API.
    [#&#8203;3882](https://togithub.com/rustwasm/wasm-bindgen/pull/3882)

-   Generate JS bindings for WebIDL dictionary setters instead of using `Reflect`. This increases the size of the Web API bindings but should be more performant. Also, importing getters/setters from JS now supports specifying the JS attribute name as a string, e.g. `#[wasm_bindgen(method, setter = "x-cdm-codecs")]`.
    [#&#8203;3898](https://togithub.com/rustwasm/wasm-bindgen/pull/3898)

-   Greatly improve the performance of sending WebIDL 'string enums' across the JavaScript boundary by converting the enum variant string to/from an int.
    [#&#8203;3915](https://togithub.com/rustwasm/wasm-bindgen/pull/3915)

-   Use `table.fill` when appropriate.
    [#&#8203;3446](https://togithub.com/rustwasm/wasm-bindgen/pull/3446)

-   Annotated methods in WebCodecs that throw.
    [#&#8203;3970](https://togithub.com/rustwasm/wasm-bindgen/pull/3970)

-   Update and stabilize the Clipboard API.
    [#&#8203;3992](https://togithub.com/rustwasm/wasm-bindgen/pull/3992)

-   Deprecate builder-pattern type setters for WebIDL dictionary types and introduce non-mutable setters instead.
    [#&#8203;3993](https://togithub.com/rustwasm/wasm-bindgen/pull/3993)

-   Allow imported async functions to return any type that can be converted from a `JsValue`.
    [#&#8203;3919](https://togithub.com/rustwasm/wasm-bindgen/pull/3919)

-   Update Web Authentication API to level 3.
    [#&#8203;4000](https://togithub.com/rustwasm/wasm-bindgen/pull/4000)

-   Deprecate `AudioBufferSourceNode.onended` and `AudioBufferSourceNode.stop()`.
    [#&#8203;4020](https://togithub.com/rustwasm/wasm-bindgen/pull/4020)

-   Increase default stack size for spawned threads from 1 to 2 MB.
    [#&#8203;3995](https://togithub.com/rustwasm/wasm-bindgen/pull/3995)

-   Deprecated parameters to `default` (`init`) and `initSync` in favor of an object.
    [#&#8203;3995](https://togithub.com/rustwasm/wasm-bindgen/pull/3995)

-   Update `AbortSignal` and `AbortController` according to the WHATWG specification.
    [#&#8203;4026](https://togithub.com/rustwasm/wasm-bindgen/pull/4026)

-   Update the Indexed DB API.
    [#&#8203;4027](https://togithub.com/rustwasm/wasm-bindgen/pull/4027)

-   `UnwrapThrowExt for Result` now makes use of the required `Debug` bound to display the error as well.
    [#&#8203;4035](https://togithub.com/rustwasm/wasm-bindgen/pull/4035)
    [#&#8203;4049](https://togithub.com/rustwasm/wasm-bindgen/pull/4049)

-   MSRV of CLI tools bumped to v1.76. This does not affect libraries like `wasm-bindgen`, `js-sys` and `web-sys`!
    [#&#8203;4037](https://togithub.com/rustwasm/wasm-bindgen/pull/4037)

-   Filtered files in published crates, significantly reducing the package size and notably excluding any bash files.
    [#&#8203;4046](https://togithub.com/rustwasm/wasm-bindgen/pull/4046)

-   Deprecated `JsStatic` in favor of `#[wasm_bindgen(thread_local)]`, which creates a `std::thread::LocalKey`. The syntax is otherwise the same.
    [#&#8203;4057](https://togithub.com/rustwasm/wasm-bindgen/pull/4057)

-   Removed `impl Deref for JsStatic` when compiling with `cfg(target_feature = "atomics")`, which was unsound.
    [#&#8203;4057](https://togithub.com/rustwasm/wasm-bindgen/pull/4057)

-   Updated the WebGPU WebIDL to the current draft as of 2024-08-05.
    [#&#8203;4062](https://togithub.com/rustwasm/wasm-bindgen/pull/4062)

-   Use object URLs for linked modules without `--split-linked-modules`.
    [#&#8203;4067](https://togithub.com/rustwasm/wasm-bindgen/pull/4067)

##### Fixed

-   Copy port from headless test server when using `WASM_BINDGEN_TEST_ADDRESS`.
    [#&#8203;3873](https://togithub.com/rustwasm/wasm-bindgen/pull/3873)

-   Fix `catch` not being thread-safe.
    [#&#8203;3879](https://togithub.com/rustwasm/wasm-bindgen/pull/3879)

-   Fix MSRV compilation.
    [#&#8203;3927](https://togithub.com/rustwasm/wasm-bindgen/pull/3927)

-   Fix `clippy::empty_docs` lint.
    [#&#8203;3946](https://togithub.com/rustwasm/wasm-bindgen/pull/3946)

-   Fix missing target features in module when enabling reference types or multi-value transformation.
    [#&#8203;3967](https://togithub.com/rustwasm/wasm-bindgen/pull/3967)

-   Fixed Rust values getting GC'd while still borrowed.
    [#&#8203;3940](https://togithub.com/rustwasm/wasm-bindgen/pull/3940)

-   Fixed Rust values not getting GC'd if they were created via. a constructor.
    [#&#8203;3940](https://togithub.com/rustwasm/wasm-bindgen/pull/3940)

-   Fix triggering `clippy::mem_forget` lint in exported structs.
    [#&#8203;3985](https://togithub.com/rustwasm/wasm-bindgen/pull/3985)

-   Fix MDN links to static interface methods.
    [#&#8203;4010](https://togithub.com/rustwasm/wasm-bindgen/pull/4010)

-   Fixed Deno support.
    [#&#8203;3990](https://togithub.com/rustwasm/wasm-bindgen/pull/3990)

-   Fix `__wbindgen_thread_destroy()` ignoring parameters.
    [#&#8203;3995](https://togithub.com/rustwasm/wasm-bindgen/pull/3995)

-   Fix `no_std` support and therefor compiling with `default-features = false`.
    [#&#8203;4005](https://togithub.com/rustwasm/wasm-bindgen/pull/4005)

-   Fix byte order for big-endian platforms.
    [#&#8203;4015](https://togithub.com/rustwasm/wasm-bindgen/pull/4015)

-   Allow ex/importing structs, functions and parameters named with raw identifiers.
    [#&#8203;4025](https://togithub.com/rustwasm/wasm-bindgen/pull/4025)

-   Implement a more reliable way to detect the stack pointer.
    [#&#8203;4036](https://togithub.com/rustwasm/wasm-bindgen/pull/4036)

-   `#[track_caller]` is now always applied on `UnwrapThrowExt` methods when not targetting `wasm32-unknown-unknown`.
    [#&#8203;4042](https://togithub.com/rustwasm/wasm-bindgen/pull/4042)

-   Fixed linked modules emitting snippet files when not using `--split-linked-modules`.
    [#&#8203;4066](https://togithub.com/rustwasm/wasm-bindgen/pull/4066)

***

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

👻 **Immortal**: This PR will be recreated if closed unmerged. Get [config help](https://togithub.com/renovatebot/renovate/discussions) if that's undesired.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOC4yNi4xIiwidXBkYXRlZEluVmVyIjoiMzguMjYuMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2024-08-19 00:23_

---

_Comment by @codspeed-hq[bot] on 2024-08-19 00:32_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/renovate/rust-wasm-bindgen-monorepo)

### Merging #12975 will **improve performances by 7.09%**

<sub>Comparing <code>renovate/rust-wasm-bindgen-monorepo</code> (4e31d94) with <code>main</code> (80ade59)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `renovate/rust-wasm-bindgen-monorepo` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 778.8 µs | 727.2 µs | +7.09% |


---

_Merged by @charliermarsh on 2024-08-19 00:44_

---

_Closed by @charliermarsh on 2024-08-19 00:44_

---

_Branch deleted on 2024-08-19 00:44_

---

_Comment by @github-actions[bot] on 2024-08-19 00:48_

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
