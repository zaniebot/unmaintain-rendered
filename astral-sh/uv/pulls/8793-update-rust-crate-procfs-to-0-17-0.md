```yaml
number: 8793
title: Update Rust crate procfs to 0.17.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/procfs-0.x
created_at: 2024-11-04T01:14:40Z
updated_at: 2024-11-04T02:15:12Z
url: https://github.com/astral-sh/uv/pull/8793
synced_at: 2026-01-10T12:00:00Z
```

# Update Rust crate procfs to 0.17.0

---

_Pull request opened by @renovate on 2024-11-04 01:14_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [procfs](https://redirect.github.com/eminence/procfs) | workspace.dependencies | minor | `0.16.0` -> `0.17.0` |

---

### Release Notes

<details>
<summary>eminence/procfs (procfs)</summary>

### [`v0.17.0`](https://redirect.github.com/eminence/procfs/releases/tag/v0.17.0)

[Compare Source](https://redirect.github.com/eminence/procfs/compare/v0.16.0...v0.17.0)

#### What's Changed

-   cgroups: skip empty controllers by [@&#8203;eliad-wiz](https://redirect.github.com/eliad-wiz) in [https://github.com/eminence/procfs/pull/292](https://redirect.github.com/eminence/procfs/pull/292)
-   Clarify CpuInfo.get_field docs by [@&#8203;eminence](https://redirect.github.com/eminence) in [https://github.com/eminence/procfs/pull/291](https://redirect.github.com/eminence/procfs/pull/291)
-   Correct copy-pasted Process::task_from_tid() doc by [@&#8203;ryoqun](https://redirect.github.com/ryoqun) in [https://github.com/eminence/procfs/pull/294](https://redirect.github.com/eminence/procfs/pull/294)
-   Ship copyright and license files with split crates by [@&#8203;michel-slm](https://redirect.github.com/michel-slm) in [https://github.com/eminence/procfs/pull/297](https://redirect.github.com/eminence/procfs/pull/297)
-   Derive serde for Pfn by [@&#8203;tatref](https://redirect.github.com/tatref) in [https://github.com/eminence/procfs/pull/301](https://redirect.github.com/eminence/procfs/pull/301)
-   Add MountInfos::iter by [@&#8203;rusty-snake](https://redirect.github.com/rusty-snake) in [https://github.com/eminence/procfs/pull/300](https://redirect.github.com/eminence/procfs/pull/300)
-   Add oom_score_adj Process methods by [@&#8203;futpib](https://redirect.github.com/futpib) in [https://github.com/eminence/procfs/pull/298](https://redirect.github.com/eminence/procfs/pull/298)
-   feat: move PhysicalPageFlags to procfs-core by [@&#8203;tatref](https://redirect.github.com/tatref) in [https://github.com/eminence/procfs/pull/303](https://redirect.github.com/eminence/procfs/pull/303)
-   Fix documentation for `Meminfo.s_reclaimable`. by [@&#8203;carletes](https://redirect.github.com/carletes) in [https://github.com/eminence/procfs/pull/306](https://redirect.github.com/eminence/procfs/pull/306)
-   Doc: Fix incorrect link to examples by [@&#8203;VxDxK](https://redirect.github.com/VxDxK) in [https://github.com/eminence/procfs/pull/307](https://redirect.github.com/eminence/procfs/pull/307)
-   Implemented proc/crypto parsing  by [@&#8203;Hwatwasthat](https://redirect.github.com/Hwatwasthat) in [https://github.com/eminence/procfs/pull/296](https://redirect.github.com/eminence/procfs/pull/296)
-   Remove lazy-static dependency by [@&#8203;notgull](https://redirect.github.com/notgull) in [https://github.com/eminence/procfs/pull/308](https://redirect.github.com/eminence/procfs/pull/308)
-   Fix build error with backtrace feature enabled by [@&#8203;mbrubeck](https://redirect.github.com/mbrubeck) in [https://github.com/eminence/procfs/pull/309](https://redirect.github.com/eminence/procfs/pull/309)
-   net: Improve parsing of /proc/net/snmp by [@&#8203;haaspors](https://redirect.github.com/haaspors) in [https://github.com/eminence/procfs/pull/313](https://redirect.github.com/eminence/procfs/pull/313)
-   Updated some examples to a bit more than printing debug. by [@&#8203;Hwatwasthat](https://redirect.github.com/Hwatwasthat) in [https://github.com/eminence/procfs/pull/314](https://redirect.github.com/eminence/procfs/pull/314)
-   Implemented crypto example by [@&#8203;Hwatwasthat](https://redirect.github.com/Hwatwasthat) in [https://github.com/eminence/procfs/pull/315](https://redirect.github.com/eminence/procfs/pull/315)
-   Add support for /dev/devices by [@&#8203;yamaura](https://redirect.github.com/yamaura) in [https://github.com/eminence/procfs/pull/311](https://redirect.github.com/eminence/procfs/pull/311)

#### New Contributors

-   [@&#8203;ryoqun](https://redirect.github.com/ryoqun) made their first contribution in [https://github.com/eminence/procfs/pull/294](https://redirect.github.com/eminence/procfs/pull/294)
-   [@&#8203;michel-slm](https://redirect.github.com/michel-slm) made their first contribution in [https://github.com/eminence/procfs/pull/297](https://redirect.github.com/eminence/procfs/pull/297)
-   [@&#8203;rusty-snake](https://redirect.github.com/rusty-snake) made their first contribution in [https://github.com/eminence/procfs/pull/300](https://redirect.github.com/eminence/procfs/pull/300)
-   [@&#8203;futpib](https://redirect.github.com/futpib) made their first contribution in [https://github.com/eminence/procfs/pull/298](https://redirect.github.com/eminence/procfs/pull/298)
-   [@&#8203;carletes](https://redirect.github.com/carletes) made their first contribution in [https://github.com/eminence/procfs/pull/306](https://redirect.github.com/eminence/procfs/pull/306)
-   [@&#8203;VxDxK](https://redirect.github.com/VxDxK) made their first contribution in [https://github.com/eminence/procfs/pull/307](https://redirect.github.com/eminence/procfs/pull/307)
-   [@&#8203;Hwatwasthat](https://redirect.github.com/Hwatwasthat) made their first contribution in [https://github.com/eminence/procfs/pull/296](https://redirect.github.com/eminence/procfs/pull/296)
-   [@&#8203;notgull](https://redirect.github.com/notgull) made their first contribution in [https://github.com/eminence/procfs/pull/308](https://redirect.github.com/eminence/procfs/pull/308)
-   [@&#8203;mbrubeck](https://redirect.github.com/mbrubeck) made their first contribution in [https://github.com/eminence/procfs/pull/309](https://redirect.github.com/eminence/procfs/pull/309)
-   [@&#8203;haaspors](https://redirect.github.com/haaspors) made their first contribution in [https://github.com/eminence/procfs/pull/313](https://redirect.github.com/eminence/procfs/pull/313)
-   [@&#8203;yamaura](https://redirect.github.com/yamaura) made their first contribution in [https://github.com/eminence/procfs/pull/311](https://redirect.github.com/eminence/procfs/pull/311)

**Full Changelog**: https://github.com/eminence/procfs/compare/v0.16.0...v0.17.0

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOC4xNDIuNCIsInVwZGF0ZWRJblZlciI6IjM4LjE0Mi40IiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2024-11-04 01:14_

---

_Merged by @charliermarsh on 2024-11-04 02:15_

---

_Closed by @charliermarsh on 2024-11-04 02:15_

---

_Branch deleted on 2024-11-04 02:15_

---
