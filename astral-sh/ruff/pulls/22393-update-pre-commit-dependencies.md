```yaml
number: 22393
title: Update pre-commit dependencies
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/pre-commit-dependencies
created_at: 2026-01-05T07:54:26Z
updated_at: 2026-01-05T09:24:42Z
url: https://github.com/astral-sh/ruff/pull/22393
synced_at: 2026-01-12T15:57:48Z
```

# Update pre-commit dependencies

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change | [Age](https://docs.renovatebot.com/merge-confidence/) | [Confidence](https://docs.renovatebot.com/merge-confidence/) |
|---|---|---|---|---|---|
| [executablebooks/mdformat](https://redirect.github.com/executablebooks/mdformat) | repository | major | `0.7.22` → `1.0.0` | ![age](https://developer.mend.io/api/mc/badges/age/github-tags/executablebooks%2fmdformat/1.0.0?slim=true) | ![confidence](https://developer.mend.io/api/mc/badges/confidence/github-tags/executablebooks%2fmdformat/0.7.22/1.0.0?slim=true) |
| [mdformat-footnote](https://redirect.github.com/gaige/mdformat-footnote) | pre-commit-python | patch | `==0.1.1` → `==0.1.2` | ![age](https://developer.mend.io/api/mc/badges/age/pypi/mdformat-footnote/0.1.2?slim=true) | ![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/mdformat-footnote/0.1.1/0.1.2?slim=true) |
| [mdformat-mkdocs](https://redirect.github.com/kyleking/mdformat-mkdocs) ([changelog](https://redirect.github.com/kyleking/mdformat-mkdocs/releases)) | pre-commit-python | major | `==4.0.0` → `==5.1.1` | ![age](https://developer.mend.io/api/mc/badges/age/pypi/mdformat-mkdocs/5.1.1?slim=true) | ![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/mdformat-mkdocs/4.0.0/5.1.1?slim=true) |

Note: The `pre-commit` manager in Renovate is not supported by the `pre-commit` maintainers or community. Please do not report any problems there, instead [create a Discussion in the Renovate repository](https://redirect.github.com/renovatebot/renovate/discussions/new) if you have any questions.

---

### Release Notes

<details>
<summary>executablebooks/mdformat (executablebooks/mdformat)</summary>

### [`v1.0.0`](https://redirect.github.com/executablebooks/mdformat/compare/0.7.22...1.0.0)

[Compare Source](https://redirect.github.com/executablebooks/mdformat/compare/0.7.22...1.0.0)

</details>

<details>
<summary>gaige/mdformat-footnote (mdformat-footnote)</summary>

### [`v0.1.2`](https://redirect.github.com/executablebooks/mdformat-footnote/releases/tag/v0.1.2)

[Compare Source](https://redirect.github.com/gaige/mdformat-footnote/compare/v0.1.1...v0.1.2)

#### What's Changed

- ⬆️ UPGRADE: Support mdformat v1 and Python >=3.10 by [@&#8203;KyleKing](https://redirect.github.com/KyleKing) in [executablebooks#14](https://redirect.github.com/executablebooks/mdformat-footnote/pull/14)
- ci: update latest ci and secure publish by [@&#8203;gaige](https://redirect.github.com/gaige) in [executablebooks#9](https://redirect.github.com/executablebooks/mdformat-footnote/pull/9)
- Revise PyPi publishing instructions in README by [@&#8203;gaige](https://redirect.github.com/gaige) in [executablebooks#15](https://redirect.github.com/executablebooks/mdformat-footnote/pull/15)

#### New Contributors

- [@&#8203;KyleKing](https://redirect.github.com/KyleKing) made their first contribution in [executablebooks#14](https://redirect.github.com/executablebooks/mdformat-footnote/pull/14)
- [@&#8203;gaige](https://redirect.github.com/gaige) made their first contribution in [executablebooks#9](https://redirect.github.com/executablebooks/mdformat-footnote/pull/9)

**Full Changelog**: <https://github.com/executablebooks/mdformat-footnote/compare/v0.1.1...v0.1.2>

</details>

<details>
<summary>kyleking/mdformat-mkdocs (mdformat-mkdocs)</summary>

### [`v5.1.1`](https://redirect.github.com/KyleKing/mdformat-mkdocs/releases/tag/v5.1.1)

[Compare Source](https://redirect.github.com/kyleking/mdformat-mkdocs/compare/v5.1.0...v5.1.1)

#### v5.1.1 (2025-11-29)

##### Fix

- correct version for mdformat-hooks

### [`v5.1.0`](https://redirect.github.com/KyleKing/mdformat-mkdocs/releases/tag/v5.1.0)

[Compare Source](https://redirect.github.com/kyleking/mdformat-mkdocs/compare/v5.0.1...v5.1.0)

**Full Changelog**: <https://github.com/KyleKing/mdformat-mkdocs/compare/v5.0.1...v5.1.0>

### [`v5.0.1`](https://redirect.github.com/KyleKing/mdformat-mkdocs/releases/tag/v5.0.1)

[Compare Source](https://redirect.github.com/kyleking/mdformat-mkdocs/compare/v5.0.0...v5.0.1)

#### What's Changed

- fix([#&#8203;45](https://redirect.github.com/kyleking/mdformat-mkdocs/issues/45)): add math support by [@&#8203;KyleKing](https://redirect.github.com/KyleKing) in [KyleKing#70](https://redirect.github.com/KyleKing/mdformat-mkdocs/pull/70)

**Full Changelog**: <https://github.com/KyleKing/mdformat-mkdocs/compare/v5.0.0...v5.0.1>

### [`v5.0.0`](https://redirect.github.com/KyleKing/mdformat-mkdocs/releases/tag/v5.0.0)

[Compare Source](https://redirect.github.com/kyleking/mdformat-mkdocs/compare/v4.5.1...v5.0.0)

**Full Changelog**: <https://github.com/KyleKing/mdformat-mkdocs/compare/v4.5.1...v5.0.0>

### [`v4.5.1`](https://redirect.github.com/KyleKing/mdformat-mkdocs/releases/tag/v4.5.1)

[Compare Source](https://redirect.github.com/kyleking/mdformat-mkdocs/compare/v4.4.2...v4.5.1)

#### What's Changed

- Fix CI virtual environment setup for tests by [@&#8203;KyleKing](https://redirect.github.com/KyleKing) in [KyleKing#69](https://redirect.github.com/KyleKing/mdformat-mkdocs/pull/69)

**Full Changelog**: <https://github.com/KyleKing/mdformat-mkdocs/compare/v4.5.0...v4.5.1>

### [`v4.4.2`](https://redirect.github.com/KyleKing/mdformat-mkdocs/releases/tag/v4.4.2)

[Compare Source](https://redirect.github.com/kyleking/mdformat-mkdocs/compare/v4.4.1...v4.4.2)

#### What's Changed

- build: replace mdformat-tables with -gfm by [@&#8203;KyleKing](https://redirect.github.com/KyleKing) in [KyleKing#61](https://redirect.github.com/KyleKing/mdformat-mkdocs/pull/61)
- fix([#&#8203;58](https://redirect.github.com/kyleking/mdformat-mkdocs/issues/58)): fix word wrap on definition lists by [@&#8203;KyleKing](https://redirect.github.com/KyleKing) in [KyleKing#62](https://redirect.github.com/KyleKing/mdformat-mkdocs/pull/62)

**Full Changelog**: <https://github.com/KyleKing/mdformat-mkdocs/compare/v4.4.1...v4.4.2>

### [`v4.4.1`](https://redirect.github.com/KyleKing/mdformat-mkdocs/releases/tag/v4.4.1)

[Compare Source](https://redirect.github.com/kyleking/mdformat-mkdocs/compare/v4.4.0...v4.4.1)

#### What's Changed

- fix([#&#8203;56](https://redirect.github.com/kyleking/mdformat-mkdocs/issues/56)): narrowly scope escape\_deflist by [@&#8203;KyleKing](https://redirect.github.com/KyleKing) in [KyleKing#57](https://redirect.github.com/KyleKing/mdformat-mkdocs/pull/57)

**Full Changelog**: <https://github.com/KyleKing/mdformat-mkdocs/compare/v4.4.0...v4.4.1>

### [`v4.4.0`](https://redirect.github.com/KyleKing/mdformat-mkdocs/releases/tag/v4.4.0)

[Compare Source](https://redirect.github.com/kyleking/mdformat-mkdocs/compare/v4.3.0...v4.4.0)

#### What's Changed

- fix([#&#8203;54](https://redirect.github.com/kyleking/mdformat-mkdocs/issues/54)): add 4-space indented deflists by [@&#8203;KyleKing](https://redirect.github.com/KyleKing) in [KyleKing#55](https://redirect.github.com/KyleKing/mdformat-mkdocs/pull/55)

**Full Changelog**: <https://github.com/KyleKing/mdformat-mkdocs/compare/v4.3.0...v4.4.0>

### [`v4.3.0`](https://redirect.github.com/KyleKing/mdformat-mkdocs/releases/tag/v4.3.0)

[Compare Source](https://redirect.github.com/kyleking/mdformat-mkdocs/compare/v4.1.2...v4.3.0)

#### What's Changed

- fix([#&#8203;45](https://redirect.github.com/kyleking/mdformat-mkdocs/issues/45)): partial fixes for wrapping attribute lists ([KyleKing@`13ad279`](https://redirect.github.com/KyleKing/mdformat-mkdocs/commit/13ad279d7456f353e5d3e799152418768cd5c0b0))
- fix([#&#8203;49](https://redirect.github.com/kyleking/mdformat-mkdocs/issues/49)): pop FILLER\_CHAR, which is causing the HTML rendering error ([KyleKing@`683b104`](https://redirect.github.com/KyleKing/mdformat-mkdocs/commit/683b104b3610b9cd80373593db229e5941eed1ca))
- feat([#&#8203;51](https://redirect.github.com/kyleking/mdformat-mkdocs/issues/51)): add caption support by [@&#8203;bittorala](https://redirect.github.com/bittorala) in [KyleKing#52](https://redirect.github.com/KyleKing/mdformat-mkdocs/pull/52)

#### New Contributors

- [@&#8203;bittorala](https://redirect.github.com/bittorala) made their first contribution in [KyleKing#52](https://redirect.github.com/KyleKing/mdformat-mkdocs/pull/52)

**Full Changelog**: <https://github.com/KyleKing/mdformat-mkdocs/compare/v4.1.2...v4.3.0>

### [`v4.1.2`](https://redirect.github.com/KyleKing/mdformat-mkdocs/releases/tag/v4.1.2)

[Compare Source](https://redirect.github.com/kyleking/mdformat-mkdocs/compare/v4.1.1...v4.1.2)

**Full Changelog**: <https://github.com/KyleKing/mdformat-mkdocs/compare/v4.1.1...v4.1.2>

### [`v4.1.1`](https://redirect.github.com/KyleKing/mdformat-mkdocs/releases/tag/v4.1.1)

[Compare Source](https://redirect.github.com/kyleking/mdformat-mkdocs/compare/v4.1.0...v4.1.1)

#### What's Changed

- fix([#&#8203;34](https://redirect.github.com/kyleking/mdformat-mkdocs/issues/34)): support pymdown snippets by [@&#8203;KyleKing](https://redirect.github.com/KyleKing) in [KyleKing#43](https://redirect.github.com/KyleKing/mdformat-mkdocs/pull/43)

**Full Changelog**: <https://github.com/KyleKing/mdformat-mkdocs/compare/v4.1.0...v4.1.1>

### [`v4.1.0`](https://redirect.github.com/KyleKing/mdformat-mkdocs/releases/tag/v4.1.0)

[Compare Source](https://redirect.github.com/kyleking/mdformat-mkdocs/compare/v4.0.0...v4.1.0)

**Full Changelog**: <https://github.com/KyleKing/mdformat-mkdocs/compare/v4.0.0...v4.1.0>

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

👻 **Immortal**: This PR will be recreated if closed unmerged. Get [config help](https://redirect.github.com/renovatebot/renovate/discussions) if that's undesired.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi42OS4xIiwidXBkYXRlZEluVmVyIjoiNDIuNjkuMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2026-01-05 07:54_

---

_Comment by @MichaReiser on 2026-01-05 09:24_

I created an upstream issue for `mdformat-mkdocs`, see https://github.com/KyleKing/mdformat-mkdocs/issues/72 

---

_Merged by @MichaReiser on 2026-01-05 09:24_

---

_Closed by @MichaReiser on 2026-01-05 09:24_

---

_Branch deleted on 2026-01-05 09:24_

---
