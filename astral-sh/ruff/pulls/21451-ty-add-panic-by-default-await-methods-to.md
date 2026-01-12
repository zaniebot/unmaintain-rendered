```yaml
number: 21451
title: "[ty] Add panic-by-default await methods to `TestServer`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/test-server-panic-by-default
created_at: 2025-11-14T13:11:44Z
updated_at: 2025-11-14T18:47:40Z
url: https://github.com/astral-sh/ruff/pull/21451
synced_at: 2026-01-12T15:57:24Z
```

# [ty] Add panic-by-default await methods to `TestServer`

---

_@MichaReiser_

## Summary

This should help with https://github.com/astral-sh/ty/issues/1551. It doesn't 
fix the root cause but I think it's an overall improvement to the testing experience.

The `await_response`, `await_notification` and `await_request` all return a `Result` today
to account for that they can fail. However, almost all writen E2E tests only care about the
case where a request is successful. 

This PR changes the `await_` methods to panic by default when they fail and adds
`try_` counterparts that return a `Result` instead for the few cases where we 
explicitly test the error case. 


I find that this overall gives better ercognomis as we get a panic 
on the line where the request failed rather than a: Test failed because request timed out
and you then have to guess which request it was.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-11-14 13:11_

---

_Label `testing` added by @MichaReiser on 2025-11-14 13:11_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-14 13:11_

---

_Label `ty` added by @MichaReiser on 2025-11-14 13:11_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-14 13:11_

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 13:13_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Review request for @sharkdp removed by @sharkdp on 2025-11-14 13:14_

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 13:15_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 44 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review request for @carljm removed by @MichaReiser on 2025-11-14 13:18_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-14 13:18_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-11-14 13:18_

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/main.rs`:391 on 2025-11-14 13:45_

Should we use the `thiserror::Error` messages here to display the error here? We could possibly add the request ID as context.

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/main.rs`:482 on 2025-11-14 13:46_

Same here although this is a bit different but the response error variant could be moved first to use `unreachable` and then a catch-all `err` for the remaining error.

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/main.rs`:375 on 2025-11-14 13:47_

I think this is still required to reference the `send_request` function in the docs.

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/main.rs`:422 on 2025-11-14 13:48_

The `send_request` requires a reference here.

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/main.rs`:586 on 2025-11-14 13:49_

Same here as the `await_response` comment.

---

_@dhruvmanila approved on 2025-11-14 13:50_

This sounds reasonable to me, I only have a few minor comments which doesn't necessarily need to be addressed.

---

_Comment by @dhruvmanila on 2025-11-14 13:54_

I believe the return type for all of the test cases can now be removed (it is `Result<()>` currently) and the dangling `Ok(())` after every test case as the panic is now part of the test server.

---

_Comment by @MichaReiser on 2025-11-14 17:38_

> I believe the return type for all of the test cases can now be removed (it is Result<()> currently) and the dangling Ok(()) after every test case as the panic is now part of the test server.

Unfortunately not. `TestServerBuilder::new` can still fail for some reason (but that's fine because the drop handler will never execute in that case)

---

_@MichaReiser reviewed on 2025-11-14 18:30_

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/main.rs`:482 on 2025-11-14 18:30_

Good idea

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 18:47_


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

_Merged by @MichaReiser on 2025-11-14 18:47_

---

_Closed by @MichaReiser on 2025-11-14 18:47_

---

_Branch deleted on 2025-11-14 18:47_

---
