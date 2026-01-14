```yaml
number: 22583
title: "fix(wasm): suppress globset debug logs in jodejs build. Fixes #22535"
type: pull_request
state: closed
author: Jkhall81
labels: []
assignees: []
base: main
head: fix/wasm-globset-leak-22535
created_at: 2026-01-14T20:25:55Z
updated_at: 2026-01-14T20:49:37Z
url: https://github.com/astral-sh/ruff/pull/22583
synced_at: 2026-01-14T21:43:07Z
```

# fix(wasm): suppress globset debug logs in jodejs build. Fixes #22535

---

_@Jkhall81_

## Summary

This PR silences internal debug output from the `globset` crate that was leaking to `stdout` in the WASM Node.js package. By adding the `release_max_level_info` feature flag to the `log` and `tracing` dependencies in `ruff_wasm`, all `debug` and `trace` level messages are stripped during compilation.

Verified by building with `wasm-pack build --target nodejs` and confirming the "built glob set" messages no longer appear in the console during execution.

Fixes #22535

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 20:28_


<!-- generated-comment typing_conformance_diagnostics_diff -->



## Typing Conformance

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 0 | 0 | +0 | ⏬ (❌) |
| False Positives | 969 | 969 | +0 | ⏬ (✅) |
| False Negatives | 1155 | 1155 | +0 | ⏬ (✅) |
| Total Diagnostics | 969 | 969 | 0 | ⏬ |
| Precision | 0.00% | 0.00% | +0.00% | ⏬ (❌) |
| Recall | 0.00% | 0.00% | +0.00% | ⏬ (❌) |


The percentage of diagnostics emitted that were expected errors held steady at 0.00%, and the percentage of expected errors that received a diagnostic held steady at 0.00%.



[Typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)



---

_Comment by @astral-sh-bot[bot] on 2026-01-14 20:29_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1825 diagnostics
+ Found 1826 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4361 diagnostics
+ Found 4359 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @MichaReiser on `crates/ruff_wasm/Cargo.toml`:37 on 2026-01-14 20:37_

I don't think this is what we want. Instead, we want to leave it to the WASM module initializer to enable logging. 

If that's not possible (requires some research), then the better solution is to gate logging behind a feature and make it optional (including the `console_log` dependency that's only there to log to the console)

---

_@MichaReiser reviewed on 2026-01-14 20:37_

---

_@Jkhall81 reviewed on 2026-01-14 20:39_

---

_Review comment by @Jkhall81 on `crates/ruff_wasm/Cargo.toml`:37 on 2026-01-14 20:39_

ok

---

_Closed by @Jkhall81 on 2026-01-14 20:39_

---

_Comment by @MichaReiser on 2026-01-14 20:46_

The ideal solution here is probably to move the following call here

https://github.com/astral-sh/ruff/blob/308373124d5e23802410a01fde10e2182c16d78a/crates/ruff_wasm/src/lib.rs#L115


Into it's own function `init_logging(level)` and make it public. If you want logging, call the function after initializing the module. If you don't want logging to be enabled, don't. Do you want to give this a try?

---

_Comment by @Jkhall81 on 2026-01-14 20:49_

Sure, I can give this a try later.

---

_Comment by @MichaReiser on 2026-01-14 20:49_

Or we do what's described here https://docs.rs/console_log/latest/console_log/#code-size

---
