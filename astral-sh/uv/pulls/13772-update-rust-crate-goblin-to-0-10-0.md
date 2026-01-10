```yaml
number: 13772
title: Update Rust crate goblin to 0.10.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/goblin-0.x
created_at: 2025-06-02T00:24:58Z
updated_at: 2025-06-02T01:57:30Z
url: https://github.com/astral-sh/uv/pull/13772
synced_at: 2026-01-10T11:10:42Z
```

# Update Rust crate goblin to 0.10.0

---

_Pull request opened by @renovate on 2025-06-02 00:24_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [goblin](https://redirect.github.com/m4b/goblin) | workspace.dependencies | minor | `0.9.0` -> `0.10.0` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>m4b/goblin (goblin)</summary>

### [`v0.10.0`](https://redirect.github.com/m4b/goblin/blob/HEAD/CHANGELOG.md#0100---2025-5-25)

[Compare Source](https://redirect.github.com/m4b/goblin/compare/0.9.3...0.10.0)

##### Breaking

build: use rust 2024 edition; bump MSRV to 1.85
pe: fix address size incompatibility on 32-bit builds, thanks [@&#8203;kkent030315](https://redirect.github.com/kkent030315): [https://github.com/m4b/goblin/pull/441](https://redirect.github.com/m4b/goblin/pull/441)
pe: fix fail on malformed certificate table parsing, thanks [@&#8203;ideeockus](https://redirect.github.com/ideeockus): [https://github.com/m4b/goblin/pull/417](https://redirect.github.com/m4b/goblin/pull/417)
pe: remove use of generics for is\_32bit, thanks [@&#8203;kkent030315](https://redirect.github.com/kkent030315): [https://github.com/m4b/goblin/pull/435](https://redirect.github.com/m4b/goblin/pull/435)
pe: Support multiple debug directories and VCFeature, Repro, ExDllCharacteristics, POGO parsers, thanks [@&#8203;kkent030315](https://redirect.github.com/kkent030315): [https://github.com/m4b/goblin/pull/403](https://redirect.github.com/m4b/goblin/pull/403)

##### Added

elf: add Loongarch macros and name mapping, thanks [@&#8203;000lbh](https://redirect.github.com/000lbh):  [https://github.com/m4b/goblin/pull/446](https://redirect.github.com/m4b/goblin/pull/446)
pe: Add base relocation parser thanks [@&#8203;kkent030315](https://redirect.github.com/kkent030315): [https://github.com/m4b/goblin/pull/444](https://redirect.github.com/m4b/goblin/pull/444)

##### Fixed

pe.header: fix parse without rich header, thanks [@&#8203;ideeockus](https://redirect.github.com/ideeockus): [https://github.com/m4b/goblin/pull/451](https://redirect.github.com/m4b/goblin/pull/451)
pe.header: fix parse header with no dos stub, thanks [@&#8203;ideeockus](https://redirect.github.com/ideeockus): [https://github.com/m4b/goblin/pull/456](https://redirect.github.com/m4b/goblin/pull/456)
pe.imports: ignore malformed imports in ParseMode::Permissive, thanks [@&#8203;ideeockus](https://redirect.github.com/ideeockus): [https://github.com/m4b/goblin/pull/442](https://redirect.github.com/m4b/goblin/pull/442)
pe: Change Section Table Real Name Handling, thanks [@&#8203;prettyroseslover](https://redirect.github.com/prettyroseslover): [https://github.com/m4b/goblin/pull/438](https://redirect.github.com/m4b/goblin/pull/438)
pe.tls: `tlsdata.parse_with_opts` - integer overflow + out of bound, thanks [@&#8203;BinFlip](https://redirect.github.com/BinFlip): [https://github.com/m4b/goblin/pull/448](https://redirect.github.com/m4b/goblin/pull/448)
pe.debug: `POGOInfo.parse_with_opts` - integer overflow + out of bound, thanks [@&#8203;BinFlip](https://redirect.github.com/BinFlip): [https://github.com/m4b/goblin/pull/449](https://redirect.github.com/m4b/goblin/pull/449)
archive: fix subtract with overflow in archive parser, thanks [@&#8203;kkent030315](https://redirect.github.com/kkent030315): [https://github.com/m4b/goblin/pull/454](https://redirect.github.com/m4b/goblin/pull/454)
te: fix subtract with overflow in TE header parser, thanks [@&#8203;kkent030315](https://redirect.github.com/kkent030315): [https://github.com/m4b/goblin/pull/452](https://redirect.github.com/m4b/goblin/pull/452)
archive: fix size overflow in name index parser, thanks [@&#8203;kkent030315](https://redirect.github.com/kkent030315): [https://github.com/m4b/goblin/pull/455](https://redirect.github.com/m4b/goblin/pull/455)
coff: fix subtract with overflow in COFF header parser, thanks [@&#8203;kkent030315](https://redirect.github.com/kkent030315): [https://github.com/m4b/goblin/pull/453](https://redirect.github.com/m4b/goblin/pull/453)

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MC4zMy42IiwidXBkYXRlZEluVmVyIjoiNDAuMzMuNiIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-06-02 00:24_

---

_Merged by @charliermarsh on 2025-06-02 01:57_

---

_Closed by @charliermarsh on 2025-06-02 01:57_

---

_Branch deleted on 2025-06-02 01:57_

---
