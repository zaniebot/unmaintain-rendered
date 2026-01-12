```yaml
number: 19760
title: "[ty] Speedup the `known_class_doesnt_fallback_to_unknown_unexpectedly_on_low_python_version` test"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/speedup-test
created_at: 2025-08-05T11:51:42Z
updated_at: 2025-08-05T11:55:52Z
url: https://github.com/astral-sh/ruff/pull/19760
synced_at: 2026-01-12T15:56:46Z
```

# [ty] Speedup the `known_class_doesnt_fallback_to_unknown_unexpectedly_on_low_python_version` test

---

_@AlexWaygood_

## Summary

This test takes >5s locally for me on `main`, which makes it our slowest unit test by far:

```
~/dev/ruff (main)⚡ % cargo test -p ty_python_semantic --lib -- known_class_doesnt_fallback_to_unknown_unexpectedly_on_low_python_version
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.09s
     Running unittests src/lib.rs (target/debug/deps/ty_python_semantic-346dd082aa08053a)

running 1 test
test types::class::tests::known_class_doesnt_fallback_to_unknown_unexpectedly_on_low_python_version ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 242 filtered out; finished in 5.30s
```

The slowness appears to be due to the fact that we're repeatedly changing the target Python version in a hot loop. This PR changes the test so that we only change the target Python version when it's actually necessary to do so, which brings the execution time of the test down to 0.39s for me locally:

```
~/dev/ruff (alex/speedup-test)⚡ % cargo test -p ty_python_semantic --lib -- known_class_doesnt_fallback_to_unknown_unexpectedly_on_low_python_version
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.12s
     Running unittests src/lib.rs (target/debug/deps/ty_python_semantic-346dd082aa08053a)

running 1 test
test types::class::tests::known_class_doesnt_fallback_to_unknown_unexpectedly_on_low_python_version ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 242 filtered out; finished in 0.39s
```

In turn, this brings the total execution time of `cargo test -p ty_python_semantic --lib` down from 6.67s to 0.54s.

## Test Plan

^See above


---

_Review requested from @carljm by @AlexWaygood on 2025-08-05 11:51_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-05 11:51_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-05 11:51_

---

_Label `testing` added by @AlexWaygood on 2025-08-05 11:51_

---

_Label `ty` added by @AlexWaygood on 2025-08-05 11:51_

---

_Comment by @github-actions[bot] on 2025-08-05 11:53_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Merged by @AlexWaygood on 2025-08-05 11:55_

---

_Closed by @AlexWaygood on 2025-08-05 11:55_

---

_Branch deleted on 2025-08-05 11:55_

---

_Comment by @github-actions[bot] on 2025-08-05 11:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---
