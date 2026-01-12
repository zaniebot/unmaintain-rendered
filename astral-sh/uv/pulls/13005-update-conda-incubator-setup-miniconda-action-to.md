```yaml
number: 13005
title: Update conda-incubator/setup-miniconda action to v3.1.1
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/conda-incubator-setup-miniconda-3.x
created_at: 2025-04-21T02:16:52Z
updated_at: 2025-04-21T12:28:54Z
url: https://github.com/astral-sh/uv/pull/13005
synced_at: 2026-01-12T16:10:30Z
```

# Update conda-incubator/setup-miniconda action to v3.1.1

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [conda-incubator/setup-miniconda](https://redirect.github.com/conda-incubator/setup-miniconda) | action | minor | `v3` -> `v3.1.1` |

---

### Release Notes

<details>
<summary>conda-incubator/setup-miniconda (conda-incubator/setup-miniconda)</summary>

### [`v3.1.1`](https://redirect.github.com/conda-incubator/setup-miniconda/blob/HEAD/CHANGELOG.md#v311-2025-01-20)

[Compare Source](https://redirect.github.com/conda-incubator/setup-miniconda/compare/v3.1.0...v3.1.1)

##### Fixes

-   [#&#8203;378]: Make `nodefaults` warning more explicit
-   [#&#8203;387]: Detect and support Linux ARM runners for both Miniconda and Miniforge

##### Tasks and Maintenance

-   [#&#8203;374]: Bump conda-incubator/setup-miniconda from 3.0.4 to 3.1.0
-   [#&#8203;375]: Bump actions/cache from 3 to 4
-   [#&#8203;384]: Bump [@&#8203;actions/tool-cache](https://redirect.github.com/actions/tool-cache) from 2.0.1 to 2.0.2
-   [#&#8203;386]: Fix link to example 14
-   [#&#8203;388]: Fix mamba 1.x examples

[v3.1.1]: https://redirect.github.com/conda-incubator/setup-miniconda/releases/tag/v3.1.1

[#&#8203;374]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/374

[#&#8203;375]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/375

[#&#8203;378]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/378

[#&#8203;384]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/384

[#&#8203;386]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/386

[#&#8203;387]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/387

[#&#8203;388]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/388

### [`v3.1.0`](https://redirect.github.com/conda-incubator/setup-miniconda/blob/HEAD/CHANGELOG.md#v310-2024-10-31)

[Compare Source](https://redirect.github.com/conda-incubator/setup-miniconda/compare/v3.0.4...v3.1.0)

##### Features

-   [#&#8203;367]: Add `conda-remove-defaults` setting to remove the `defaults` channel
    if added implicitly
-   [#&#8203;342]: Add `installation-dir` to customize where the installers are installed
    to
-   [#&#8203;328]: Make conda's cache configurable via `pkgs-dirs`

##### Fixes

-   [#&#8203;360]: Start deprecation of `miniforge-variant: Mambaforge`
-   [#&#8203;362]: Ignore conda cygpath warning
-   [#&#8203;368]: Address mamba v2 incompatibilities
-   [#&#8203;350]: set `CONDA` environment variable regardless of useBundled option

##### Tasks and Maintenance

-   [#&#8203;348]: Bump conda-incubator/setup-miniconda from 3.0.3 to 3.0.4
-   [#&#8203;353]: Bump semver and [@&#8203;types/semver](https://redirect.github.com/types/semver)
-   [#&#8203;356]: Bump braces from 3.0.2 to 3.0.3
-   [#&#8203;359]: Bump semver from 7.6.2 to 7.6.3
-   [#&#8203;370]: Bump [@&#8203;actions/core](https://redirect.github.com/actions/core) from 1.10.1 to 1.11.1

[v3.1.0]: https://redirect.github.com/conda-incubator/setup-miniconda/releases/tag/v3.1.0

[#&#8203;360]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/360

[#&#8203;362]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/362

[#&#8203;368]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/368

[#&#8203;367]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/367

[#&#8203;342]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/342

[#&#8203;328]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/328

[#&#8203;350]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/350

[#&#8203;348]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/348

[#&#8203;353]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/353

[#&#8203;356]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/356

[#&#8203;359]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/359

[#&#8203;370]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/370

### [`v3.0.4`](https://redirect.github.com/conda-incubator/setup-miniconda/blob/HEAD/CHANGELOG.md#v304-2024-04-25)

[Compare Source](https://redirect.github.com/conda-incubator/setup-miniconda/compare/v3.0.3...v3.0.4)

##### Fixes

-   [#&#8203;345] Fix running on macOS 13 on Intel since the runners no longer provide
    miniconda by default.

##### Tasks and Maintenance

-   [#&#8203;337] Bump conda-incubator/setup-miniconda from 3.0.2 to 3.0.3 ([#&#8203;337](https://redirect.github.com/conda-incubator/setup-miniconda/issues/337))
-   [#&#8203;338] Bump normalize-url from 8.0.0 to 8.0.1
-   [#&#8203;340] Bump undici from 5.27.4 to 5.28.5

[v3.0.4]: https://redirect.github.com/conda-incubator/setup-miniconda/releases/tag/v3.0.4

[#&#8203;337]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/337

[#&#8203;338]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/338

[#&#8203;340]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/340

[#&#8203;345]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/345

### [`v3.0.3`](https://redirect.github.com/conda-incubator/setup-miniconda/blob/HEAD/CHANGELOG.md#v303-2024-02-27)

[Compare Source](https://redirect.github.com/conda-incubator/setup-miniconda/compare/v3.0.2...v3.0.3)

##### Fixes

-   [#&#8203;336] Fall back to miniconda3 latest when no bundled version + empty with
    params

##### Tasks and Maintenance

-   [#&#8203;335] Bump conda-incubator/setup-miniconda from 3.0.1 to 3.0.2

[v3.0.3]: https://redirect.github.com/conda-incubator/setup-miniconda/releases/tag/v3.0.3

[#&#8203;335]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/335

[#&#8203;336]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/336

### [`v3.0.2`](https://redirect.github.com/conda-incubator/setup-miniconda/blob/HEAD/CHANGELOG.md#v302-2024-02-22)

[Compare Source](https://redirect.github.com/conda-incubator/setup-miniconda/compare/v3.0.1...v3.0.2)

##### Fixes

-   [#&#8203;312] Enable ARM64 on macOS for Miniforge and Mambaforge including automatic
    architecture detection.

##### Tasks and Maintenance

-   [#&#8203;327] Bump conda-incubator/setup-miniconda from 3.0.0 to 3.0.1
-   [#&#8203;330] Bump actions/cache from 3 to 4
-   [#&#8203;334] Bump undici from 5.27.2 to 5.28.3

[v3.0.2]: https://redirect.github.com/conda-incubator/setup-miniconda/releases/tag/v3.0.2

[#&#8203;312]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/312

[#&#8203;327]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/327

[#&#8203;330]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/330

[#&#8203;334]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/334

### [`v3.0.1`](https://redirect.github.com/conda-incubator/setup-miniconda/blob/HEAD/CHANGELOG.md#v301-2023-11-29)

[Compare Source](https://redirect.github.com/conda-incubator/setup-miniconda/compare/v3...v3.0.1)

##### Fixes

-   [#&#8203;325] Fix environment activation on windows (a v3 regression) due to
    hard-coded install PATH

[v3.0.1]: https://redirect.github.com/conda-incubator/setup-miniconda/releases/tag/v3.0.1

[#&#8203;325]: https://redirect.github.com/conda-incubator/setup-miniconda/pull/325

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4yNDguNCIsInVwZGF0ZWRJblZlciI6IjM5LjI0OC40IiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-04-21 02:16_

---

_Merged by @charliermarsh on 2025-04-21 12:28_

---

_Closed by @charliermarsh on 2025-04-21 12:28_

---

_Branch deleted on 2025-04-21 12:28_

---
