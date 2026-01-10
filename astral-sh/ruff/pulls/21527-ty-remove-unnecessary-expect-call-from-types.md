```yaml
number: 21527
title: "[ty] Remove unnecessary `.expect()` call from `types/instance.rs`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/isprotocol-crash
created_at: 2025-11-19T14:40:33Z
updated_at: 2025-11-19T19:26:38Z
url: https://github.com/astral-sh/ruff/pull/21527
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Remove unnecessary `.expect()` call from `types/instance.rs`

---

_Pull request opened by @AlexWaygood on 2025-11-19 14:40_

## Summary

The `.expect()` call here:

https://github.com/astral-sh/ruff/blob/5dd56264fb4e394c9919cf897cab39f52ccba124/crates/ty_python_semantic/src/types/instance.rs#L816-L827

is the direct cause of the panic in https://github.com/astral-sh/ty/issues/1587. This patch gets rid of the panic by refactoring our `Protocol` enum so that the `Protocol::FromClass` variant holds a `ProtocolClass` instance rather than a `ClassType` instance (all the `.expect()` call was doing was attempting to convert form a `ClassType` to a `ProtocolClass`).

I hoped that this would provide a fix for https://github.com/astral-sh/ty/issues/1587, but we still panic on the provided reproducible examples in that issue even with this PR. Nonetheless, I think this PR is a worthwhile change to make because:
- It's probably slightly more efficient this way (we no longer have to re-verify that the wrapped class in a `Protocol::FromClass()` variant is a protocol class every time we want to access its interface)
- It's nice to get rid of `.expect()` calls where possible, and this one seems definitely unnecessary
- The _new_ panic message on this PR branch makes it much clearer what the underlying cause of the bug in https://github.com/astral-sh/ty/issues/1587 is:

    <details>
    <summary>New panic message</summary>

    ```
	error[panic]: Panicked at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a885bb4/src/function/execute.rs:321:21 when checking `/Users/alexw/dev/ruff/foo.py`: `ClassLiteral < 'db >::explicit_bases_(Id(4c09)): execute: too many cycle iterations`
	info: This indicates a bug in ty.
	info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
	info: Platform: macos aarch64
	info: Version: ruff/0.14.5+60 (18a14bfaf 2025-11-19)
	info: Args: ["target/debug/ty", "check", "foo.py", "--python-version=3.14"]
	info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
	info: query stacktrace:
	   0: cached_protocol_interface(Id(6805))
	             at crates/ty_python_semantic/src/types/protocol_class.rs:790
	   1: is_equivalent_to_object_inner(Id(8003))
	             at crates/ty_python_semantic/src/types/instance.rs:667
	   2: infer_deferred_types(Id(1409))
	             at crates/ty_python_semantic/src/types/infer.rs:141
	             cycle heads: infer_definition_types(Id(140b)) -> iteration = 200, TypeVarInstance < 'db >::lazy_bound_(Id(5803)) -> iteration = 200
	   3: TypeVarInstance < 'db >::lazy_bound_(Id(5802))
	             at crates/ty_python_semantic/src/types.rs:8734
	   4: infer_definition_types(Id(140c))
	             at crates/ty_python_semantic/src/types/infer.rs:94
	   5: infer_deferred_types(Id(140a))
	             at crates/ty_python_semantic/src/types/infer.rs:141
	   6: TypeVarInstance < 'db >::lazy_bound_(Id(5803))
	             at crates/ty_python_semantic/src/types.rs:8734
	   7: infer_definition_types(Id(140b))
	             at crates/ty_python_semantic/src/types/infer.rs:94
	   8: infer_scope_types(Id(1000))
	             at crates/ty_python_semantic/src/types/infer.rs:70
	   9: check_file_impl(Id(c00))
	             at crates/ty_project/src/lib.rs:535
	
	
	Found 1 diagnostic
	WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
    ```

    </details>

## Test Plan

All existing tests pass.


---

_Review requested from @carljm by @AlexWaygood on 2025-11-19 14:40_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-19 14:40_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-19 14:40_

---

_Label `internal` added by @AlexWaygood on 2025-11-19 14:40_

---

_Label `ty` added by @AlexWaygood on 2025-11-19 14:40_

---

_Comment by @astral-sh-bot[bot] on 2025-11-19 14:43_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-19 14:43_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_@carljm approved on 2025-11-19 19:23_

---

_Merged by @AlexWaygood on 2025-11-19 19:26_

---

_Closed by @AlexWaygood on 2025-11-19 19:26_

---

_Branch deleted on 2025-11-19 19:26_

---
