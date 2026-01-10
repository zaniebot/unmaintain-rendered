```yaml
number: 11293
title: Update Rust crate libc to v0.2.154
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/libc-0.x-lockfile
created_at: 2024-05-06T00:58:45Z
updated_at: 2024-05-06T01:18:54Z
url: https://github.com/astral-sh/ruff/pull/11293
synced_at: 2026-01-10T22:37:02Z
```

# Update Rust crate libc to v0.2.154

---

_Pull request opened by @renovate on 2024-05-06 00:58_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [libc](https://togithub.com/rust-lang/libc) | workspace.dependencies | patch | `0.2.153` -> `0.2.154` |

---

### Release Notes

<details>
<summary>rust-lang/libc (libc)</summary>

### [`v0.2.154`](https://togithub.com/rust-lang/libc/releases/tag/0.2.154)

[Compare Source](https://togithub.com/rust-lang/libc/compare/0.2.153...0.2.154)

#### What's Changed

-   Fix CI on v0.2 by [@&#8203;JohnTitor](https://togithub.com/JohnTitor) in [https://github.com/rust-lang/libc/pull/3589](https://togithub.com/rust-lang/libc/pull/3589)
-   \[Backport [#&#8203;3547](https://togithub.com/rust-lang/libc/issues/3547)] Add ioctl FS_IOC\_{G,S}{ETVERSION,ETFLAGS} for CSKY by [@&#8203;Dirreke](https://togithub.com/Dirreke) in [https://github.com/rust-lang/libc/pull/3572](https://togithub.com/rust-lang/libc/pull/3572)
-   Add Linux riscv64 HWCAP defines (libc-0.2) by [@&#8203;Xeonacid](https://togithub.com/Xeonacid) in [https://github.com/rust-lang/libc/pull/3580](https://togithub.com/rust-lang/libc/pull/3580)
-   Add missing MIPS R6 FS_IOC_\* definitions by [@&#8203;chenx97](https://togithub.com/chenx97) in [https://github.com/rust-lang/libc/pull/3591](https://togithub.com/rust-lang/libc/pull/3591)
-   Support posix_spawn on Android by [@&#8203;pcc](https://togithub.com/pcc) in [https://github.com/rust-lang/libc/pull/3602](https://togithub.com/rust-lang/libc/pull/3602)
-   \[0.2] Fix libc-tests for loongarch64 by [@&#8203;heiher](https://togithub.com/heiher) in [https://github.com/rust-lang/libc/pull/3607](https://togithub.com/rust-lang/libc/pull/3607)
-   visionOS Support by [@&#8203;agg23](https://togithub.com/agg23) in [https://github.com/rust-lang/libc/pull/3568](https://togithub.com/rust-lang/libc/pull/3568)
-   \[0.2] linux/musl: Add support for LoongArch64 by [@&#8203;heiher](https://togithub.com/heiher) in [https://github.com/rust-lang/libc/pull/3606](https://togithub.com/rust-lang/libc/pull/3606)
-   v0.2: Fix c_char on AIX by [@&#8203;taiki-e](https://togithub.com/taiki-e) in [https://github.com/rust-lang/libc/pull/3662](https://togithub.com/rust-lang/libc/pull/3662)
-   solarish adding SO_EXCLBIND constant. by [@&#8203;devnexen](https://togithub.com/devnexen) in [https://github.com/rust-lang/libc/pull/3651](https://togithub.com/rust-lang/libc/pull/3651)
-   \[0.2] Add SIG constants to espidf by [@&#8203;Tevz-Beskovnik](https://togithub.com/Tevz-Beskovnik) in [https://github.com/rust-lang/libc/pull/3658](https://togithub.com/rust-lang/libc/pull/3658)
-   add all android sysconf constants by [@&#8203;fkm3](https://togithub.com/fkm3) in [https://github.com/rust-lang/libc/pull/3656](https://togithub.com/rust-lang/libc/pull/3656)
-   feat: more \_PC_XXX constants for apple targets by [@&#8203;SteveLauC](https://togithub.com/SteveLauC) in [https://github.com/rust-lang/libc/pull/3649](https://togithub.com/rust-lang/libc/pull/3649)
-   feat: O_EXEC/O_SEARCH for apple platforms by [@&#8203;SteveLauC](https://togithub.com/SteveLauC) in [https://github.com/rust-lang/libc/pull/3668](https://togithub.com/rust-lang/libc/pull/3668)
-   \[0.2] Add constant AT_MINSIGSTKSZ by [@&#8203;ur4t](https://togithub.com/ur4t) in [https://github.com/rust-lang/libc/pull/3637](https://togithub.com/rust-lang/libc/pull/3637)
-   Haiku: synchronize with post R1-beta 4 changes in libc by [@&#8203;nielx](https://togithub.com/nielx) in [https://github.com/rust-lang/libc/pull/3638](https://togithub.com/rust-lang/libc/pull/3638)
-   adding getentropy/getrandom to dragonflybsd. by [@&#8203;devnexen](https://togithub.com/devnexen) in [https://github.com/rust-lang/libc/pull/3618](https://togithub.com/rust-lang/libc/pull/3618)
-   Move strftime, strftime_l, strptime to linux_like by [@&#8203;pcc](https://togithub.com/pcc) in [https://github.com/rust-lang/libc/pull/3600](https://togithub.com/rust-lang/libc/pull/3600)
-   update crate version to 0.2.154 by [@&#8203;Dirreke](https://togithub.com/Dirreke) in [https://github.com/rust-lang/libc/pull/3573](https://togithub.com/rust-lang/libc/pull/3573)

#### New Contributors

-   [@&#8203;pcc](https://togithub.com/pcc) made their first contribution in [https://github.com/rust-lang/libc/pull/3602](https://togithub.com/rust-lang/libc/pull/3602)
-   [@&#8203;agg23](https://togithub.com/agg23) made their first contribution in [https://github.com/rust-lang/libc/pull/3568](https://togithub.com/rust-lang/libc/pull/3568)
-   [@&#8203;Tevz-Beskovnik](https://togithub.com/Tevz-Beskovnik) made their first contribution in [https://github.com/rust-lang/libc/pull/3658](https://togithub.com/rust-lang/libc/pull/3658)
-   [@&#8203;ur4t](https://togithub.com/ur4t) made their first contribution in [https://github.com/rust-lang/libc/pull/3637](https://togithub.com/rust-lang/libc/pull/3637)

**Full Changelog**: https://github.com/rust-lang/libc/compare/0.2.153...0.2.154

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

This PR has been generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View repository job log [here](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzNy4zNDAuMTAiLCJ1cGRhdGVkSW5WZXIiOiIzNy4zNDAuMTAiLCJ0YXJnZXRCcmFuY2giOiJtYWluIiwibGFiZWxzIjpbImludGVybmFsIl19-->


---

_Label `internal` added by @renovate[bot] on 2024-05-06 00:58_

---

_Merged by @charliermarsh on 2024-05-06 01:08_

---

_Closed by @charliermarsh on 2024-05-06 01:08_

---

_Branch deleted on 2024-05-06 01:08_

---

_Comment by @github-actions[bot] on 2024-05-06 01:18_

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
