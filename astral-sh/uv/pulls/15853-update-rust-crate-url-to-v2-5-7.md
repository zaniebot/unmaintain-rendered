```yaml
number: 15853
title: Update Rust crate url to v2.5.7
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/url-2.x-lockfile
created_at: 2025-09-15T00:46:22Z
updated_at: 2025-09-15T01:19:45Z
url: https://github.com/astral-sh/uv/pull/15853
synced_at: 2026-01-12T16:11:58Z
```

# Update Rust crate url to v2.5.7

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [url](https://redirect.github.com/servo/rust-url) | workspace.dependencies | patch | `2.5.4` -> `2.5.7` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>servo/rust-url (url)</summary>

### [`v2.5.7`](https://redirect.github.com/servo/rust-url/releases/tag/v2.5.7)

#### What's Changed

##### v.2.5.6

- Bump versions by [@&#8203;valenting](https://redirect.github.com/valenting) in [#&#8203;1062](https://redirect.github.com/servo/rust-url/pull/1062)
- version bump to 2.5.6 by [@&#8203;edgul](https://redirect.github.com/edgul) in [#&#8203;1065](https://redirect.github.com/servo/rust-url/pull/1065)
- Use no-std instead of no\_std in Cargo.toml by [@&#8203;valenting](https://redirect.github.com/valenting) in [#&#8203;1064](https://redirect.github.com/servo/rust-url/pull/1064)
- fix: add Serde std feature if needed by [@&#8203;reneca](https://redirect.github.com/reneca) in [#&#8203;1068](https://redirect.github.com/servo/rust-url/pull/1068)

##### v.2.5.7

- make serde std feature use optional syntax by [@&#8203;klensy](https://redirect.github.com/klensy) in [#&#8203;1071](https://redirect.github.com/servo/rust-url/pull/1071)

#### New Contributors

- [@&#8203;reneca](https://redirect.github.com/reneca) made their first contribution in [#&#8203;1068](https://redirect.github.com/servo/rust-url/pull/1068)

**Full Changelog**: <https://github.com/servo/rust-url/compare/v2.5.5...v2.5.7>

### [`v2.5.5`](https://redirect.github.com/servo/rust-url/releases/tag/v2.5.5)

[Compare Source](https://redirect.github.com/servo/rust-url/compare/v2.5.4...v2.5.5)

#### What's Changed

- ci: downgrade crates when building for Rust 1.67.0 by [@&#8203;mxinden](https://redirect.github.com/mxinden) in [#&#8203;1003](https://redirect.github.com/servo/rust-url/pull/1003)
- ci: run unit tests with sanitizers by [@&#8203;mxinden](https://redirect.github.com/mxinden) in [#&#8203;1002](https://redirect.github.com/servo/rust-url/pull/1002)
- fix small typo by [@&#8203;hkBst](https://redirect.github.com/hkBst) in [#&#8203;1011](https://redirect.github.com/servo/rust-url/pull/1011)
- chore: fix clippy errors on main by [@&#8203;dsherret](https://redirect.github.com/dsherret) in [#&#8203;1019](https://redirect.github.com/servo/rust-url/pull/1019)
- perf: remove heap allocation in parse\_query by [@&#8203;dsherret](https://redirect.github.com/dsherret) in [#&#8203;1020](https://redirect.github.com/servo/rust-url/pull/1020)
- perf: slightly improve parsing a port by [@&#8203;dsherret](https://redirect.github.com/dsherret) in [#&#8203;1022](https://redirect.github.com/servo/rust-url/pull/1022)
- perf: improve to\_file\_path() by [@&#8203;dsherret](https://redirect.github.com/dsherret) in [#&#8203;1018](https://redirect.github.com/servo/rust-url/pull/1018)
- perf: make parse\_scheme slightly faster by [@&#8203;dsherret](https://redirect.github.com/dsherret) in [#&#8203;1025](https://redirect.github.com/servo/rust-url/pull/1025)
- update LICENSE-MIT by [@&#8203;wmjae](https://redirect.github.com/wmjae) in [#&#8203;1029](https://redirect.github.com/servo/rust-url/pull/1029)
- perf: url encode path segments in longer string slices by [@&#8203;dsherret](https://redirect.github.com/dsherret) in [#&#8203;1026](https://redirect.github.com/servo/rust-url/pull/1026)
- Disable the default features on serde by [@&#8203;rilipco](https://redirect.github.com/rilipco) in [#&#8203;1033](https://redirect.github.com/servo/rust-url/pull/1033)
- docs: base url relative join by [@&#8203;tisonkun](https://redirect.github.com/tisonkun) in [#&#8203;1013](https://redirect.github.com/servo/rust-url/pull/1013)
- perf: remove heap allocation in parse\_host by [@&#8203;dsherret](https://redirect.github.com/dsherret) in [#&#8203;1021](https://redirect.github.com/servo/rust-url/pull/1021)
- Update tests to Unicode 16.0 by [@&#8203;hsivonen](https://redirect.github.com/hsivonen) in [#&#8203;1045](https://redirect.github.com/servo/rust-url/pull/1045)
- Add some some basic functions to `Mime` by [@&#8203;mrobinson](https://redirect.github.com/mrobinson) in [#&#8203;1047](https://redirect.github.com/servo/rust-url/pull/1047)
- ran `cargo clippy --fix -- -Wclippy::use_self` by [@&#8203;mrobinson](https://redirect.github.com/mrobinson) in [#&#8203;1048](https://redirect.github.com/servo/rust-url/pull/1048)
- Fix MSRV and clippy CI by [@&#8203;Manishearth](https://redirect.github.com/Manishearth) in [#&#8203;1058](https://redirect.github.com/servo/rust-url/pull/1058)
- Update `Url::domain` docs to show that it includes subdomain by [@&#8203;supercoolspy](https://redirect.github.com/supercoolspy) in [#&#8203;1057](https://redirect.github.com/servo/rust-url/pull/1057)
- set\_hostname should error when encountering colon ':' by [@&#8203;edgul](https://redirect.github.com/edgul) in [#&#8203;1060](https://redirect.github.com/servo/rust-url/pull/1060)
- version bump to 2.5.5 by [@&#8203;edgul](https://redirect.github.com/edgul) in [#&#8203;1061](https://redirect.github.com/servo/rust-url/pull/1061)

#### New Contributors

- [@&#8203;mxinden](https://redirect.github.com/mxinden) made their first contribution in [#&#8203;1003](https://redirect.github.com/servo/rust-url/pull/1003)
- [@&#8203;hkBst](https://redirect.github.com/hkBst) made their first contribution in [#&#8203;1011](https://redirect.github.com/servo/rust-url/pull/1011)
- [@&#8203;wmjae](https://redirect.github.com/wmjae) made their first contribution in [#&#8203;1029](https://redirect.github.com/servo/rust-url/pull/1029)
- [@&#8203;rilipco](https://redirect.github.com/rilipco) made their first contribution in [#&#8203;1033](https://redirect.github.com/servo/rust-url/pull/1033)
- [@&#8203;tisonkun](https://redirect.github.com/tisonkun) made their first contribution in [#&#8203;1013](https://redirect.github.com/servo/rust-url/pull/1013)
- [@&#8203;supercoolspy](https://redirect.github.com/supercoolspy) made their first contribution in [#&#8203;1057](https://redirect.github.com/servo/rust-url/pull/1057)

**Full Changelog**: <https://github.com/servo/rust-url/compare/v2.5.4...v2.5.5>

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

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS45Ny4xMCIsInVwZGF0ZWRJblZlciI6IjQxLjk3LjEwIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-09-15 00:46_

---

_Merged by @charliermarsh on 2025-09-15 01:19_

---

_Closed by @charliermarsh on 2025-09-15 01:19_

---

_Branch deleted on 2025-09-15 01:19_

---
