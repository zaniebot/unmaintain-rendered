```yaml
number: 19522
title: "[ty] Move server tests as integration tests"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
  - testing
  - ty
assignees: []
merged: true
base: main
head: dhruv/move-server-tests-as-integration
created_at: 2025-07-24T06:44:11Z
updated_at: 2025-07-24T16:27:47Z
url: https://github.com/astral-sh/ruff/pull/19522
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Move server tests as integration tests

---

_Pull request opened by @dhruvmanila on 2025-07-24 06:44_

## Summary

Reference: https://github.com/astral-sh/ruff/pull/19391#discussion_r2222780892

---

_Review requested from @carljm by @dhruvmanila on 2025-07-24 06:44_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-07-24 06:44_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-07-24 06:44_

---

_Label `server` added by @dhruvmanila on 2025-07-24 06:44_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-07-24 06:44_

---

_Review requested from @dcreager by @dhruvmanila on 2025-07-24 06:44_

---

_Label `testing` added by @dhruvmanila on 2025-07-24 06:44_

---

_Label `ty` added by @dhruvmanila on 2025-07-24 06:44_

---

_Review request for @dcreager removed by @dhruvmanila on 2025-07-24 06:44_

---

_Review request for @carljm removed by @dhruvmanila on 2025-07-24 06:44_

---

_Review request for @sharkdp removed by @dhruvmanila on 2025-07-24 06:44_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2025-07-24 06:44_

---

_@dhruvmanila reviewed on 2025-07-24 06:45_

---

_Review comment by @dhruvmanila on `crates/ty_server/Cargo.toml`:44 on 2025-07-24 06:45_

I believe there's a better way than doing this?

---

_Comment by @github-actions[bot] on 2025-07-24 06:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser reviewed on 2025-07-24 08:39_

---

_Review comment by @MichaReiser on `crates/ty_server/Cargo.toml`:44 on 2025-07-24 08:39_

Hmm that's weird. This shouldn't need changing.

---

_@MichaReiser reviewed on 2025-07-24 08:39_

---

_Review comment by @MichaReiser on `crates/ty_server/src/lib.rs`:19 on 2025-07-24 08:39_

I would move this implementation into the `test` folder too. See how I shared the comment `CliTest` infrastructure in `ty/tests/cli`

---

_Review comment by @dhruvmanila on `crates/ty_server/src/lib.rs`:19 on 2025-07-24 13:35_

I've pushed this change. I've moved the tests under `e2e/` directory and moved the `TestServer` and related data structures to `e2e/main.rs` file.

---

_@dhruvmanila reviewed on 2025-07-24 13:35_

---

_Review comment by @dhruvmanila on `crates/ty_server/Cargo.toml`:50 on 2025-07-24 13:36_

This is still required because we conditionally add the `serde::Serialize` attribute to the `ClientOptions` and other nested structs.

---

_@dhruvmanila reviewed on 2025-07-24 13:36_

---

_@dhruvmanila reviewed on 2025-07-24 13:36_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/lib.rs`:9 on 2025-07-24 13:36_

These are required by the mock server implementation.

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-07-24 13:37_

---

_Comment by @github-actions[bot] on 2025-07-24 13:45_

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

_Review comment by @MichaReiser on `crates/ty_server/Cargo.toml`:50 on 2025-07-24 14:01_

Could we use `assert_debug_snapshot` instead so that we don't need the serde serialization?

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/main.rs`:113 on 2025-07-24 14:03_

Does the `TestServer` need to be public. Aren't all tests in the same package (I don't know)

---

_@MichaReiser approved on 2025-07-24 14:03_

Thank you

---

_@dhruvmanila reviewed on 2025-07-24 15:22_

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/main.rs`:113 on 2025-07-24 15:22_

No, it does not, was a leftover from previous version. I've made it `pub(crate)`.

---

_@dhruvmanila reviewed on 2025-07-24 15:22_

---

_Review comment by @dhruvmanila on `crates/ty_server/Cargo.toml`:50 on 2025-07-24 15:22_

That is not pretty ;)

---

_Merged by @dhruvmanila on 2025-07-24 16:10_

---

_Closed by @dhruvmanila on 2025-07-24 16:10_

---

_Branch deleted on 2025-07-24 16:10_

---

_@MichaReiser reviewed on 2025-07-24 16:11_

---

_Review comment by @MichaReiser on `crates/ty_server/Cargo.toml`:50 on 2025-07-24 16:11_

Is it? Isn't it almost the same as JSON (unless we add some manual implementations):

https://insta.rs/docs/serializers/#debug

---

_@MichaReiser reviewed on 2025-07-24 16:12_

---

_Review comment by @MichaReiser on `crates/ty_server/Cargo.toml`:50 on 2025-07-24 16:12_

I would also be fine to simply always derive `Serialize` for those types. It's not that many and it simplifies the crate setup

---

_@dhruvmanila reviewed on 2025-07-24 16:27_

---

_Review comment by @dhruvmanila on `crates/ty_server/Cargo.toml`:50 on 2025-07-24 16:27_

Yeah, I think implementing `Serialize` unconditionally seems fine, I'll do it as a quick follow up tomorrow morning.

---
