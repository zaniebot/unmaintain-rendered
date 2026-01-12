```yaml
number: 12266
title: "[`flake8-async`] Update `ASYNC116` to match upstream"
type: pull_request
state: merged
author: augustelalande
labels:
  - preview
assignees: []
merged: true
base: main
head: async116
created_at: 2024-07-10T04:49:48Z
updated_at: 2024-07-10T15:39:22Z
url: https://github.com/astral-sh/ruff/pull/12266
synced_at: 2026-01-12T15:55:40Z
```

# [`flake8-async`] Update `ASYNC116` to match upstream

---

_@augustelalande_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Update the name of `ASYNC116` to match [upstream](https://flake8-async.readthedocs.io/en/latest/rules.html).

Also update the functionality to match upstream by adding support for `anyio` (gated behind preview). `asyncio` does not have a `sleep_forever` equivalent.

## Test Plan

Added tests for `anyio`


---

_@charliermarsh approved on 2024-07-10 04:58_

Excellent, thanks.

---

_@charliermarsh reviewed on 2024-07-10 04:58_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_async/rules/long_sleep_not_forever.rs`:111 on 2024-07-10 04:58_

Maybe we extract this simultaneously with the `!matches!(qualified_name.segments(), ["trio" | "anyio", "sleep"])` above, to make the `unwrap` unnecessary?

---

_@charliermarsh reviewed on 2024-07-10 04:59_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_async/rules/long_sleep_not_forever.rs`:112 on 2024-07-10 04:59_

I think we have an `.is_disabled()` method.

---

_@augustelalande reviewed on 2024-07-10 05:38_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/flake8_async/rules/long_sleep_not_forever.rs`:112 on 2024-07-10 05:38_

Done

---

_@augustelalande reviewed on 2024-07-10 05:39_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/flake8_async/rules/long_sleep_not_forever.rs`:111 on 2024-07-10 05:39_

I'm not sure what pattern you're suggesting

---

_Comment by @codspeed-hq[bot] on 2024-07-10 05:43_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/augustelalande:async116)

### Merging #12266 will **not alter performance**

<sub>Comparing <code>augustelalande:async116</code> (e5b65d8) with <code>main</code> (d365f1a)</sub>



### Summary

`✅ 33` untouched benchmarks






---

_@zanieb reviewed on 2024-07-10 06:11_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_async/rules/long_sleep_not_forever.rs`:111 on 2024-07-10 06:11_

Maybe something like

```rust
let module = match qualfied_name.segments() {
   ["trio", "sleep"] => AsyncModule::Trio,
   ["anyio", "sleep"] => AsyncModle::Anyio,
   _ => return;
};
```

or

```rust
let Some(module) = AsyncModule::try_from(&qualified_name) else { return; };
if !matches!(qualified_name.segments(), [_, "sleep"]) { return; };
```

---

_Label `preview` added by @MichaReiser on 2024-07-10 07:24_

---

_Merged by @MichaReiser on 2024-07-10 07:58_

---

_Closed by @MichaReiser on 2024-07-10 07:58_

---

_Branch deleted on 2024-07-10 15:39_

---
