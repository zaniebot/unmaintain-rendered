```yaml
number: 12758
title: Update Rust crate tokio to v1.44.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
  - security
assignees: []
merged: true
base: main
head: renovate/crate-tokio-vulnerability
created_at: 2025-04-08T19:44:55Z
updated_at: 2025-04-08T19:57:07Z
url: https://github.com/astral-sh/uv/pull/12758
synced_at: 2026-01-10T11:10:40Z
```

# Update Rust crate tokio to v1.44.0

---

_Pull request opened by @renovate on 2025-04-08 19:44_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [tokio](https://tokio.rs) ([source](https://redirect.github.com/tokio-rs/tokio)) | dev-dependencies | minor | `1.43.1` -> `1.44.0` |
| [tokio](https://tokio.rs) ([source](https://redirect.github.com/tokio-rs/tokio)) | workspace.dependencies | minor | `1.43.1` -> `1.44.0` |

### GitHub Vulnerability Alerts

#### [GHSA-rr8g-9fpq-6wmg](https://redirect.github.com/tokio-rs/tokio/pull/7232)

The broadcast channel internally calls `clone` on the stored value when receiving it, and only requires `T:Send`. This means that using the broadcast channel with values that are `Send` but not `Sync` can trigger unsoundness if the `clone` implementation makes use of the value being `!Sync`.

Thank you to Austin Bonander for finding and reporting this issue.

---

### Release Notes

<details>
<summary>tokio-rs/tokio (tokio)</summary>

### [`v1.44.0`](https://redirect.github.com/tokio-rs/tokio/releases/tag/tokio-1.44.0): Tokio v1.44.0

[Compare Source](https://redirect.github.com/tokio-rs/tokio/compare/tokio-1.43.1...tokio-1.44.0)

##### 1.44.0 (March 7th, 2025)

This release changes the `from_std` method on sockets to panic if a blocking socket is provided. We determined this change is not a breaking change as Tokio is not intended to operate using blocking sockets. Doing so results in runtime hangs and should be considered a bug. Accidentally passing a blocking socket to Tokio is one of the most common user mistakes. If this change causes an issue for you, please comment on [#&#8203;7172].

##### Added

-   coop: add `task::coop` module ([#&#8203;7116])
-   process: add `Command::get_kill_on_drop()` ([#&#8203;7086])
-   sync: add `broadcast::Sender::closed` ([#&#8203;6685], [#&#8203;7090])
-   sync: add `broadcast::WeakSender` ([#&#8203;7100])
-   sync: add `oneshot::Receiver::is_empty()` ([#&#8203;7153])
-   sync: add `oneshot::Receiver::is_terminated()` ([#&#8203;7152])

##### Fixed

-   fs: empty reads on `File` should not start a background read ([#&#8203;7139])
-   process: calling `start_kill` on exited child should not fail ([#&#8203;7160])
-   signal: fix `CTRL_CLOSE`, `CTRL_LOGOFF`, `CTRL_SHUTDOWN` on windows ([#&#8203;7122])
-   sync: properly handle panic during mpsc drop ([#&#8203;7094])

##### Changes

-   runtime: clean up magic number in registration set ([#&#8203;7112])
-   coop: make coop yield using waker defer strategy ([#&#8203;7185])
-   macros: make `select!` budget-aware ([#&#8203;7164])
-   net: panic when passing a blocking socket to `from_std` ([#&#8203;7166])
-   io: clean up buffer casts ([#&#8203;7142])

##### Changes to unstable APIs

-   rt: add before and after task poll callbacks ([#&#8203;7120])
-   tracing: make the task tracing API unstable public ([#&#8203;6972])

##### Documented

-   docs: fix nesting of sections in top-level docs ([#&#8203;7159])
-   fs: rename symlink and hardlink parameter names ([#&#8203;7143])
-   io: swap reader/writer in simplex doc test ([#&#8203;7176])
-   macros: docs about `select!` alternatives ([#&#8203;7110])
-   net: rename the argument for `send_to` ([#&#8203;7146])
-   process: add example for reading `Child` stdout ([#&#8203;7141])
-   process: clarify `Child::kill` behavior ([#&#8203;7162])
-   process: fix grammar of the `ChildStdin` struct doc comment ([#&#8203;7192])
-   runtime: consistently use `worker_threads` instead of `core_threads` ([#&#8203;7186])

[#&#8203;6685]: https://redirect.github.com/tokio-rs/tokio/pull/6685

[#&#8203;6972]: https://redirect.github.com/tokio-rs/tokio/pull/6972

[#&#8203;7086]: https://redirect.github.com/tokio-rs/tokio/pull/7086

[#&#8203;7090]: https://redirect.github.com/tokio-rs/tokio/pull/7090

[#&#8203;7094]: https://redirect.github.com/tokio-rs/tokio/pull/7094

[#&#8203;7100]: https://redirect.github.com/tokio-rs/tokio/pull/7100

[#&#8203;7110]: https://redirect.github.com/tokio-rs/tokio/pull/7110

[#&#8203;7112]: https://redirect.github.com/tokio-rs/tokio/pull/7112

[#&#8203;7116]: https://redirect.github.com/tokio-rs/tokio/pull/7116

[#&#8203;7120]: https://redirect.github.com/tokio-rs/tokio/pull/7120

[#&#8203;7122]: https://redirect.github.com/tokio-rs/tokio/pull/7122

[#&#8203;7139]: https://redirect.github.com/tokio-rs/tokio/pull/7139

[#&#8203;7141]: https://redirect.github.com/tokio-rs/tokio/pull/7141

[#&#8203;7142]: https://redirect.github.com/tokio-rs/tokio/pull/7142

[#&#8203;7143]: https://redirect.github.com/tokio-rs/tokio/pull/7143

[#&#8203;7146]: https://redirect.github.com/tokio-rs/tokio/pull/7146

[#&#8203;7152]: https://redirect.github.com/tokio-rs/tokio/pull/7152

[#&#8203;7153]: https://redirect.github.com/tokio-rs/tokio/pull/7153

[#&#8203;7159]: https://redirect.github.com/tokio-rs/tokio/pull/7159

[#&#8203;7160]: https://redirect.github.com/tokio-rs/tokio/pull/7160

[#&#8203;7162]: https://redirect.github.com/tokio-rs/tokio/pull/7162

[#&#8203;7164]: https://redirect.github.com/tokio-rs/tokio/pull/7164

[#&#8203;7166]: https://redirect.github.com/tokio-rs/tokio/pull/7166

[#&#8203;7172]: https://redirect.github.com/tokio-rs/tokio/pull/7172

[#&#8203;7176]: https://redirect.github.com/tokio-rs/tokio/pull/7176

[#&#8203;7185]: https://redirect.github.com/tokio-rs/tokio/pull/7185

[#&#8203;7186]: https://redirect.github.com/tokio-rs/tokio/pull/7186

[#&#8203;7192]: https://redirect.github.com/tokio-rs/tokio/pull/7192

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about these updates again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4yMzUuMiIsInVwZGF0ZWRJblZlciI6IjM5LjIzNS4yIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCIsInNlY3VyaXR5Il19-->


---

_Label `internal` added by @renovate[bot] on 2025-04-08 19:44_

---

_Label `security` added by @renovate[bot] on 2025-04-08 19:44_

---

_Merged by @charliermarsh on 2025-04-08 19:57_

---

_Closed by @charliermarsh on 2025-04-08 19:57_

---

_Branch deleted on 2025-04-08 19:57_

---
