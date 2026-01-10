```yaml
number: 11567
title: Update Rust crate codspeed-criterion-compat to v2.8.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/codspeed-criterion-compat-2.x-lockfile
created_at: 2025-02-17T01:51:47Z
updated_at: 2025-02-17T02:49:16Z
url: https://github.com/astral-sh/uv/pull/11567
synced_at: 2026-01-10T11:10:38Z
```

# Update Rust crate codspeed-criterion-compat to v2.8.0

---

_Pull request opened by @renovate on 2025-02-17 01:51_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [codspeed-criterion-compat](https://codspeed.io) ([source](https://redirect.github.com/CodSpeedHQ/codspeed-rust)) | dependencies | minor | `2.7.2` -> `2.8.0` |

---

### Release Notes

<details>
<summary>CodSpeedHQ/codspeed-rust (codspeed-criterion-compat)</summary>

### [`v2.8.0`](https://redirect.github.com/CodSpeedHQ/codspeed-rust/releases/tag/v2.8.0)

[Compare Source](https://redirect.github.com/CodSpeedHQ/codspeed-rust/compare/v2.7.2...v2.8.0)

#### What's Changed

This introduces Divan compatibility layer and also Wall Time support. Check out the documentation to try it out [here](https://docs.codspeed.io/benchmarks/rust/divan).

##### Details

-   ci: bump actions/checkout to v4 by [@&#8203;fargito](https://redirect.github.com/fargito) in [https://github.com/CodSpeedHQ/codspeed-rust/pull/56](https://redirect.github.com/CodSpeedHQ/codspeed-rust/pull/56)
-   docs: simplify rust benchmarks definition by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia) in [https://github.com/CodSpeedHQ/codspeed-rust/pull/44](https://redirect.github.com/CodSpeedHQ/codspeed-rust/pull/44)
-   Support walltime runs with divan by [@&#8203;art049](https://redirect.github.com/art049) in [https://github.com/CodSpeedHQ/codspeed-rust/pull/66](https://redirect.github.com/CodSpeedHQ/codspeed-rust/pull/66)
-   Make `cargo-codspeed` build targets to different directories between walltime and instrumented by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [https://github.com/CodSpeedHQ/codspeed-rust/pull/68](https://redirect.github.com/CodSpeedHQ/codspeed-rust/pull/68)
-   feat: make codspeed raw results in the walltime directory as well by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [https://github.com/CodSpeedHQ/codspeed-rust/pull/70](https://redirect.github.com/CodSpeedHQ/codspeed-rust/pull/70)
-   chore: add an internal divan fork by [@&#8203;art049](https://redirect.github.com/art049) in [https://github.com/CodSpeedHQ/codspeed-rust/pull/69](https://redirect.github.com/CodSpeedHQ/codspeed-rust/pull/69)
-   Add codspeed<>divan compat layer by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [https://github.com/CodSpeedHQ/codspeed-rust/pull/65](https://redirect.github.com/CodSpeedHQ/codspeed-rust/pull/65)
-   fix: only show walltime collection warning when appropriate by [@&#8203;art049](https://redirect.github.com/art049) in [https://github.com/CodSpeedHQ/codspeed-rust/pull/71](https://redirect.github.com/CodSpeedHQ/codspeed-rust/pull/71)
-   feat(divan_compat): support types and manage types and args in codspeed uri by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [https://github.com/CodSpeedHQ/codspeed-rust/pull/72](https://redirect.github.com/CodSpeedHQ/codspeed-rust/pull/72)
-   feat: add some TheAlgorithm benches by [@&#8203;art049](https://redirect.github.com/art049) in [https://github.com/CodSpeedHQ/codspeed-rust/pull/73](https://redirect.github.com/CodSpeedHQ/codspeed-rust/pull/73)
-   Add divan_compat msrv check in CI by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [https://github.com/CodSpeedHQ/codspeed-rust/pull/74](https://redirect.github.com/CodSpeedHQ/codspeed-rust/pull/74)
-   feat: add readme to divan_compat by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [https://github.com/CodSpeedHQ/codspeed-rust/pull/75](https://redirect.github.com/CodSpeedHQ/codspeed-rust/pull/75)

#### New Contributors

-   [@&#8203;fargito](https://redirect.github.com/fargito) made their first contribution in [https://github.com/CodSpeedHQ/codspeed-rust/pull/56](https://redirect.github.com/CodSpeedHQ/codspeed-rust/pull/56)

#### New Contributors

-   [@&#8203;fargito](https://redirect.github.com/fargito) made their first contribution in [https://github.com/CodSpeedHQ/codspeed-rust/pull/56](https://redirect.github.com/CodSpeedHQ/codspeed-rust/pull/56)

**Full Changelog**: https://github.com/CodSpeedHQ/codspeed-rust/compare/v2.7.2...v2.8.0

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xNjcuMSIsInVwZGF0ZWRJblZlciI6IjM5LjE2Ny4xIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-02-17 01:51_

---

_Merged by @charliermarsh on 2025-02-17 02:49_

---

_Closed by @charliermarsh on 2025-02-17 02:49_

---

_Branch deleted on 2025-02-17 02:49_

---
