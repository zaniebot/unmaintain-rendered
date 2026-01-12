```yaml
number: 11891
title: Update Rust crate url to v2.5.1
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/url-2.x-lockfile
created_at: 2024-06-17T01:23:16Z
updated_at: 2024-06-17T05:54:53Z
url: https://github.com/astral-sh/ruff/pull/11891
synced_at: 2026-01-12T15:55:39Z
```

# Update Rust crate url to v2.5.1

---

_@renovate_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [url](https://togithub.com/servo/rust-url) | workspace.dependencies | patch | `2.5.0` -> `2.5.1` |

---

### Release Notes

<details>
<summary>servo/rust-url (url)</summary>

### [`v2.5.1`](https://togithub.com/servo/rust-url/releases/tag/v2.5.1)

[Compare Source](https://togithub.com/servo/rust-url/compare/v2.5.0...v2.5.1)

#### What's Changed

-   Be more detailed in documentation of set_query by [@&#8203;philippeitis](https://togithub.com/philippeitis) in [https://github.com/servo/rust-url/pull/737](https://togithub.com/servo/rust-url/pull/737)
-   perf(punycode): avoid double allocation in decode_to_string by [@&#8203;bishopcheckmate](https://togithub.com/bishopcheckmate) in [https://github.com/servo/rust-url/pull/894](https://togithub.com/servo/rust-url/pull/894)
-   Use SPECIAL_PATH_SEGMENT when encoding path in from_file_path by [@&#8203;valenting](https://togithub.com/valenting) in [https://github.com/servo/rust-url/pull/902](https://togithub.com/servo/rust-url/pull/902)
-   Add dependabot by [@&#8203;oriontvv](https://togithub.com/oriontvv) in [https://github.com/servo/rust-url/pull/903](https://togithub.com/servo/rust-url/pull/903)
-   Bump codecov/codecov-action from 3 to 4 by [@&#8203;dependabot](https://togithub.com/dependabot) in [https://github.com/servo/rust-url/pull/904](https://togithub.com/servo/rust-url/pull/904)
-   Bump actions/upload-artifact from 2 to 4 by [@&#8203;dependabot](https://togithub.com/dependabot) in [https://github.com/servo/rust-url/pull/905](https://togithub.com/servo/rust-url/pull/905)
-   Bump actions/checkout from 3 to 4 by [@&#8203;dependabot](https://togithub.com/dependabot) in [https://github.com/servo/rust-url/pull/906](https://togithub.com/servo/rust-url/pull/906)
-   Fix non-base64 data URLs with % characters not followed by hex digits by [@&#8203;SmaugPool](https://togithub.com/SmaugPool) in [https://github.com/servo/rust-url/pull/797](https://togithub.com/servo/rust-url/pull/797)
-   Rename `master` branch to `main` by [@&#8203;mrobinson](https://togithub.com/mrobinson) in [https://github.com/servo/rust-url/pull/914](https://togithub.com/servo/rust-url/pull/914)
-   Add bench for to_ascii on an already-Punycode name by [@&#8203;hsivonen](https://togithub.com/hsivonen) in [https://github.com/servo/rust-url/pull/915](https://togithub.com/servo/rust-url/pull/915)
-   Update URLs by [@&#8203;atouchet](https://togithub.com/atouchet) in [https://github.com/servo/rust-url/pull/916](https://togithub.com/servo/rust-url/pull/916)
-   Fix lint by [@&#8203;valenting](https://togithub.com/valenting) in [https://github.com/servo/rust-url/pull/920](https://togithub.com/servo/rust-url/pull/920)
-   Fix multiple issues on wasm32, and runs url tests in CI by [@&#8203;micolous](https://togithub.com/micolous) in [https://github.com/servo/rust-url/pull/886](https://togithub.com/servo/rust-url/pull/886)
-   Non-special URLs can have their paths erased by [@&#8203;DylanOToole2](https://togithub.com/DylanOToole2) in [https://github.com/servo/rust-url/pull/921](https://togithub.com/servo/rust-url/pull/921)
-   docs: document SyntaxViolation variants, remove bare URLs by [@&#8203;aatifsyed](https://togithub.com/aatifsyed) in [https://github.com/servo/rust-url/pull/924](https://togithub.com/servo/rust-url/pull/924)
-   docs: Document possible replacements of the base URL by [@&#8203;mo8it](https://togithub.com/mo8it) in [https://github.com/servo/rust-url/pull/926](https://togithub.com/servo/rust-url/pull/926)
-   Reimplement idna on top of ICU4X by [@&#8203;hsivonen](https://togithub.com/hsivonen) in [https://github.com/servo/rust-url/pull/923](https://togithub.com/servo/rust-url/pull/923)

#### New Contributors

-   [@&#8203;philippeitis](https://togithub.com/philippeitis) made their first contribution in [https://github.com/servo/rust-url/pull/737](https://togithub.com/servo/rust-url/pull/737)
-   [@&#8203;bishopcheckmate](https://togithub.com/bishopcheckmate) made their first contribution in [https://github.com/servo/rust-url/pull/894](https://togithub.com/servo/rust-url/pull/894)
-   [@&#8203;oriontvv](https://togithub.com/oriontvv) made their first contribution in [https://github.com/servo/rust-url/pull/903](https://togithub.com/servo/rust-url/pull/903)
-   [@&#8203;dependabot](https://togithub.com/dependabot) made their first contribution in [https://github.com/servo/rust-url/pull/904](https://togithub.com/servo/rust-url/pull/904)
-   [@&#8203;SmaugPool](https://togithub.com/SmaugPool) made their first contribution in [https://github.com/servo/rust-url/pull/797](https://togithub.com/servo/rust-url/pull/797)
-   [@&#8203;hsivonen](https://togithub.com/hsivonen) made their first contribution in [https://github.com/servo/rust-url/pull/915](https://togithub.com/servo/rust-url/pull/915)
-   [@&#8203;micolous](https://togithub.com/micolous) made their first contribution in [https://github.com/servo/rust-url/pull/886](https://togithub.com/servo/rust-url/pull/886)
-   [@&#8203;DylanOToole2](https://togithub.com/DylanOToole2) made their first contribution in [https://github.com/servo/rust-url/pull/921](https://togithub.com/servo/rust-url/pull/921)
-   [@&#8203;aatifsyed](https://togithub.com/aatifsyed) made their first contribution in [https://github.com/servo/rust-url/pull/924](https://togithub.com/servo/rust-url/pull/924)
-   [@&#8203;mo8it](https://togithub.com/mo8it) made their first contribution in [https://github.com/servo/rust-url/pull/926](https://togithub.com/servo/rust-url/pull/926)

**Full Changelog**: https://github.com/servo/rust-url/compare/v2.5.0...v2.5.1

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzNy4zOTMuMCIsInVwZGF0ZWRJblZlciI6IjM3LjM5My4wIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2024-06-17 01:23_

---

_Merged by @MichaReiser on 2024-06-17 05:54_

---

_Closed by @MichaReiser on 2024-06-17 05:54_

---

_Branch deleted on 2024-06-17 05:54_

---
