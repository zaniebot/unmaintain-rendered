```yaml
number: 12830
title: Update dependency mdformat-mkdocs to v3
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/mdformat-mkdocs-3.x
created_at: 2024-08-12T02:03:04Z
updated_at: 2024-08-12T04:24:31Z
url: https://github.com/astral-sh/ruff/pull/12830
synced_at: 2026-01-10T21:38:32Z
```

# Update dependency mdformat-mkdocs to v3

---

_Pull request opened by @renovate on 2024-08-12 02:03_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [mdformat-mkdocs](https://togithub.com/kyleking/mdformat-mkdocs) ([changelog](https://togithub.com/kyleking/mdformat-mkdocs/releases)) | `==2.0.4` -> `==3.0.0` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/mdformat-mkdocs/3.0.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/mdformat-mkdocs/3.0.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/mdformat-mkdocs/2.0.4/3.0.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/mdformat-mkdocs/2.0.4/3.0.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

### Release Notes

<details>
<summary>kyleking/mdformat-mkdocs (mdformat-mkdocs)</summary>

### [`v3.0.0`](https://togithub.com/KyleKing/mdformat-mkdocs/releases/tag/v3.0.0)

[Compare Source](https://togithub.com/kyleking/mdformat-mkdocs/compare/v2.1.1...v3.0.0)

##### What's Changed

-   refactor([#&#8203;25](https://togithub.com/kyleking/mdformat-mkdocs/issues/25)): support anchor links as a plugin in [https://github.com/KyleKing/mdformat-mkdocs/pull/30](https://togithub.com/KyleKing/mdformat-mkdocs/pull/30)
    -   fix([#&#8203;33](https://togithub.com/kyleking/mdformat-mkdocs/issues/33)): render anchor links above a heading without newlines in https://github.com/KyleKing/mdformat-mkdocs/commit/7c1e4892f5117649268729e884dbd46ba40e49a7 and https://github.com/KyleKing/mdformat-mkdocs/commit/4be7ca86afaf02b96827cd3fa4410734a1eb11fa
-   refactor!: rename according to syntax source (e.g. `material_*`, `mkdocs_*`, `pymd_*` (python markdown), `mkdocstrings_*`) in https://github.com/KyleKing/mdformat-mkdocs/commit/d6c465aa584788ddaf2957d3b6aec294910531da
-   feat: render HTML for cross-references in https://github.com/KyleKing/mdformat-mkdocs/commit/a967d20c4955a2063904154f95b86f779b4f4dde
-   ci: major improvements from template (https://github.com/KyleKing/mdformat-plugin-template)

**Full Changelog**: https://github.com/KyleKing/mdformat-mkdocs/compare/v2.1.1...v3.0.0

### [`v2.1.1`](https://togithub.com/KyleKing/mdformat-mkdocs/releases/tag/v2.1.1)

[Compare Source](https://togithub.com/kyleking/mdformat-mkdocs/compare/v2.1.0...v2.1.1)

##### What's Changed

-   fix([#&#8203;31](https://togithub.com/kyleking/mdformat-mkdocs/issues/31)): ignore HTML within Code Blocks by [@&#8203;KyleKing](https://togithub.com/KyleKing) in [https://github.com/KyleKing/mdformat-mkdocs/pull/32](https://togithub.com/KyleKing/mdformat-mkdocs/pull/32)

**Full Changelog**: https://github.com/KyleKing/mdformat-mkdocs/compare/v2.1.0...v2.1.1

### [`v2.1.0`](https://togithub.com/KyleKing/mdformat-mkdocs/releases/tag/v2.1.0)

[Compare Source](https://togithub.com/kyleking/mdformat-mkdocs/compare/v2.0.11...v2.1.0)

##### What's Changed

-   feat([#&#8203;28](https://togithub.com/kyleking/mdformat-mkdocs/issues/28)): support "Abbreviations" by [@&#8203;KyleKing](https://togithub.com/KyleKing) in [https://github.com/KyleKing/mdformat-mkdocs/pull/29](https://togithub.com/KyleKing/mdformat-mkdocs/pull/29)

**Full Changelog**: https://github.com/KyleKing/mdformat-mkdocs/compare/v2.0.11...v2.1.0

### [`v2.0.11`](https://togithub.com/KyleKing/mdformat-mkdocs/releases/tag/v2.0.11)

[Compare Source](https://togithub.com/kyleking/mdformat-mkdocs/compare/v2.0.10...v2.0.11)

##### Changes

-   fix([#&#8203;25](https://togithub.com/kyleking/mdformat-mkdocs/issues/25)): add support for "[markdown anchors](https://mkdocstrings.github.io/autorefs/#markdown-anchors)" syntax from the `mkdocs` [autorefs](https://mkdocstrings.github.io/autorefs) plugin

**Full Changelog**: https://github.com/KyleKing/mdformat-mkdocs/compare/v2.0.10...v2.0.11

### [`v2.0.10`](https://togithub.com/KyleKing/mdformat-mkdocs/releases/tag/v2.0.10)

[Compare Source](https://togithub.com/kyleking/mdformat-mkdocs/compare/v2.0.9...v2.0.10)

Changes:

-   fix([#&#8203;24](https://togithub.com/kyleking/mdformat-mkdocs/issues/24)): respect ordered lists that start with `0.` ([#&#8203;26](https://togithub.com/kyleking/mdformat-mkdocs/issues/26))

**Full Changelog**: https://github.com/KyleKing/mdformat-mkdocs/compare/v2.0.9...v2.0.10

### [`v2.0.9`](https://togithub.com/KyleKing/mdformat-mkdocs/releases/tag/v2.0.9)

[Compare Source](https://togithub.com/kyleking/mdformat-mkdocs/compare/v2.0.8...v2.0.9)

Changelog:

-   fix([#&#8203;23](https://togithub.com/kyleking/mdformat-mkdocs/issues/23)): ignore empty newlines when in fenced code blocks

**Full Changelog**: https://github.com/KyleKing/mdformat-mkdocs/compare/v2.0.8...v2.0.9

### [`v2.0.8`](https://togithub.com/KyleKing/mdformat-mkdocs/releases/tag/v2.0.8)

[Compare Source](https://togithub.com/kyleking/mdformat-mkdocs/compare/v2.0.7...v2.0.8)

Changelog:

-   Fix([#&#8203;21](https://togithub.com/kyleking/mdformat-mkdocs/issues/21)): ignore lists in fenced code

**Full Changelog**: https://github.com/KyleKing/mdformat-mkdocs/compare/v2.0.7...v2.0.8

### [`v2.0.7`](https://togithub.com/KyleKing/mdformat-mkdocs/releases/tag/v2.0.7)

[Compare Source](https://togithub.com/kyleking/mdformat-mkdocs/compare/v2.0.6...v2.0.7)

Changelog:

-   Fix([#&#8203;20](https://togithub.com/kyleking/mdformat-mkdocs/issues/20)): https://github.com/KyleKing/mdformat-mkdocs/commit/01a6916f41fb82d3e0f840d79882acb0181563af

**Full Changelog**: https://github.com/KyleKing/mdformat-mkdocs/compare/v2.0.6...v2.0.7

### [`v2.0.6`](https://togithub.com/KyleKing/mdformat-mkdocs/releases/tag/v2.0.6)

[Compare Source](https://togithub.com/kyleking/mdformat-mkdocs/compare/v2.0.5...v2.0.6)

##### Changelog

-   Resolve typo in CLI for [#&#8203;19](https://togithub.com/kyleking/mdformat-mkdocs/issues/19) (https://github.com/KyleKing/mdformat-mkdocs/commit/3dc80a03f4572ca701ef7b55aa75f0484018dde9)
-   Make `mdformat-wikilink` optional thanks to a quick release ([https://github.com/tmr232/mdformat-wikilink/issues/6](https://togithub.com/tmr232/mdformat-wikilink/issues/6))!

**Full Changelog**: https://github.com/KyleKing/mdformat-mkdocs/compare/v2.0.5...v2.0.6

### [`v2.0.5`](https://togithub.com/KyleKing/mdformat-mkdocs/releases/tag/v2.0.5)

[Compare Source](https://togithub.com/kyleking/mdformat-mkdocs/compare/v2.0.4...v2.0.5)

Changelog:

-   Resolves [#&#8203;19](https://togithub.com/kyleking/mdformat-mkdocs/issues/19). Add `--ignore-missing-references` to prevent escaping brackets for compatibility with python mkdocstrings
-   feat: back-port `mdformat-wikilink` to Python 3.8 by default (see: [https://github.com/tmr232/mdformat-wikilink/issues/6](https://togithub.com/tmr232/mdformat-wikilink/issues/6))

**Full Changelog**: https://github.com/KyleKing/mdformat-mkdocs/compare/v2.0.5...v2.0.5

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

This PR was generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOC4yMC4xIiwidXBkYXRlZEluVmVyIjoiMzguMjAuMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2024-08-12 02:03_

---

_Comment by @dhruvmanila on 2024-08-12 04:24_

I don't see how the breaking change in `mdformat-mkdocs` (https://github.com/KyleKing/mdformat-mkdocs/commit/d6c465aa584788ddaf2957d3b6aec294910531da) should affect us. I built the docs locally and everything seems to be fine.

The breaking change is to update the following import and its references:
```diff
- from mdformat_mkdocs.mdit_plugins import mkdocs_admon_plugin
+ from mdformat_mkdocs.mdit_plugins import material_admon_plugin
```

---

_Merged by @dhruvmanila on 2024-08-12 04:24_

---

_Closed by @dhruvmanila on 2024-08-12 04:24_

---

_Branch deleted on 2024-08-12 04:24_

---
