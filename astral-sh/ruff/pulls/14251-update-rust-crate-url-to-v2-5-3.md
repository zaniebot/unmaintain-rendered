```yaml
number: 14251
title: Update Rust crate url to v2.5.3
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/url-2.x-lockfile
created_at: 2024-11-11T00:21:07Z
updated_at: 2024-11-11T00:47:16Z
url: https://github.com/astral-sh/ruff/pull/14251
synced_at: 2026-01-12T15:55:47Z
```

# Update Rust crate url to v2.5.3

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [url](https://redirect.github.com/servo/rust-url) | workspace.dependencies | patch | `2.5.2` -> `2.5.3` |

---

### Release Notes

<details>
<summary>servo/rust-url (url)</summary>

### [`v2.5.3`](https://redirect.github.com/servo/rust-url/releases/tag/v2.5.3)

[Compare Source](https://redirect.github.com/servo/rust-url/compare/v2.5.2...v2.5.3)

##### What's Changed

-   fix: enable wasip2 feature for wasm32-wasip2 target by [@&#8203;brooksmtownsend](https://redirect.github.com/brooksmtownsend) in [https://github.com/servo/rust-url/pull/960](https://redirect.github.com/servo/rust-url/pull/960)
-   Fix idna tests with no_std by [@&#8203;cjwatson](https://redirect.github.com/cjwatson) in [https://github.com/servo/rust-url/pull/963](https://redirect.github.com/servo/rust-url/pull/963)
-   Fix debugger_visualizer test failures. by [@&#8203;valenting](https://redirect.github.com/valenting) in [https://github.com/servo/rust-url/pull/967](https://redirect.github.com/servo/rust-url/pull/967)
-   Add AsciiSet::EMPTY and boolean operators by [@&#8203;joshka](https://redirect.github.com/joshka) in [https://github.com/servo/rust-url/pull/969](https://redirect.github.com/servo/rust-url/pull/969)
-   mention why we pin unicode-width by [@&#8203;Manishearth](https://redirect.github.com/Manishearth) in [https://github.com/servo/rust-url/pull/972](https://redirect.github.com/servo/rust-url/pull/972)
-   refactor and add tests for percent encoding by [@&#8203;joshka](https://redirect.github.com/joshka) in [https://github.com/servo/rust-url/pull/977](https://redirect.github.com/servo/rust-url/pull/977)
-   Add a test for and fix issue [#&#8203;974](https://redirect.github.com/servo/rust-url/issues/974) by [@&#8203;hansl](https://redirect.github.com/hansl) in [https://github.com/servo/rust-url/pull/975](https://redirect.github.com/servo/rust-url/pull/975)
-   `no_std` support for the `url` crate by [@&#8203;domenukk](https://redirect.github.com/domenukk) in [https://github.com/servo/rust-url/pull/831](https://redirect.github.com/servo/rust-url/pull/831)
-   Normalize URL paths: convert /.//p, /..//p, and //p to p by [@&#8203;theskim](https://redirect.github.com/theskim) in [https://github.com/servo/rust-url/pull/943](https://redirect.github.com/servo/rust-url/pull/943)
-   support Hermit by [@&#8203;m-mueller678](https://redirect.github.com/m-mueller678) in [https://github.com/servo/rust-url/pull/985](https://redirect.github.com/servo/rust-url/pull/985)
-   fix: support `wasm32-wasip2` on the stable channel by [@&#8203;brooksmtownsend](https://redirect.github.com/brooksmtownsend) in [https://github.com/servo/rust-url/pull/983](https://redirect.github.com/servo/rust-url/pull/983)
-   Improve serde error output by [@&#8203;konstin](https://redirect.github.com/konstin) in [https://github.com/servo/rust-url/pull/982](https://redirect.github.com/servo/rust-url/pull/982)
-   OSS-Fuzz: Add more fuzzer by [@&#8203;arthurscchan](https://redirect.github.com/arthurscchan) in [https://github.com/servo/rust-url/pull/988](https://redirect.github.com/servo/rust-url/pull/988)
-   Merge idna-v1x to main by [@&#8203;hsivonen](https://redirect.github.com/hsivonen) in [https://github.com/servo/rust-url/pull/990](https://redirect.github.com/servo/rust-url/pull/990)

##### New Contributors

-   [@&#8203;brooksmtownsend](https://redirect.github.com/brooksmtownsend) made their first contribution in [https://github.com/servo/rust-url/pull/960](https://redirect.github.com/servo/rust-url/pull/960)
-   [@&#8203;cjwatson](https://redirect.github.com/cjwatson) made their first contribution in [https://github.com/servo/rust-url/pull/963](https://redirect.github.com/servo/rust-url/pull/963)
-   [@&#8203;joshka](https://redirect.github.com/joshka) made their first contribution in [https://github.com/servo/rust-url/pull/969](https://redirect.github.com/servo/rust-url/pull/969)
-   [@&#8203;hansl](https://redirect.github.com/hansl) made their first contribution in [https://github.com/servo/rust-url/pull/975](https://redirect.github.com/servo/rust-url/pull/975)
-   [@&#8203;theskim](https://redirect.github.com/theskim) made their first contribution in [https://github.com/servo/rust-url/pull/943](https://redirect.github.com/servo/rust-url/pull/943)
-   [@&#8203;m-mueller678](https://redirect.github.com/m-mueller678) made their first contribution in [https://github.com/servo/rust-url/pull/985](https://redirect.github.com/servo/rust-url/pull/985)
-   [@&#8203;konstin](https://redirect.github.com/konstin) made their first contribution in [https://github.com/servo/rust-url/pull/982](https://redirect.github.com/servo/rust-url/pull/982)
-   [@&#8203;arthurscchan](https://redirect.github.com/arthurscchan) made their first contribution in [https://github.com/servo/rust-url/pull/988](https://redirect.github.com/servo/rust-url/pull/988)

**Full Changelog**: https://github.com/servo/rust-url/compare/v2.5.2...v2.5.3

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS43LjEiLCJ1cGRhdGVkSW5WZXIiOiIzOS43LjEiLCJ0YXJnZXRCcmFuY2giOiJtYWluIiwibGFiZWxzIjpbImludGVybmFsIl19-->


---

_Label `internal` added by @renovate[bot] on 2024-11-11 00:21_

---

_Comment by @github-actions[bot] on 2024-11-11 00:43_

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

_Merged by @charliermarsh on 2024-11-11 00:47_

---

_Closed by @charliermarsh on 2024-11-11 00:47_

---

_Branch deleted on 2024-11-11 00:47_

---
