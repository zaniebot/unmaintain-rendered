```yaml
number: 22255
title: Update dependency ruff to v0.14.10
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2025-12-29T16:11:59Z
updated_at: 2025-12-29T16:23:45Z
url: https://github.com/astral-sh/ruff/pull/22255
synced_at: 2026-01-10T16:36:18Z
```

# Update dependency ruff to v0.14.10

---

_Pull request opened by @renovate on 2025-12-29 16:11_

This PR contains the following updates:

| Package | Change | [Age](https://docs.renovatebot.com/merge-confidence/) | [Confidence](https://docs.renovatebot.com/merge-confidence/) |
|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.14.7` -> `==0.14.10` | ![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.14.10?slim=true) | ![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.14.7/0.14.10?slim=true) |

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.14.10`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#01410)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.14.9...0.14.10)

Released on 2025-12-18.

##### Preview features

- \[formatter] Fluent formatting of method chains ([#&#8203;21369](https://redirect.github.com/astral-sh/ruff/pull/21369))
- \[formatter] Keep lambda parameters on one line and parenthesize the body if it expands ([#&#8203;21385](https://redirect.github.com/astral-sh/ruff/pull/21385))
- \[`flake8-implicit-str-concat`] New rule to prevent implicit string concatenation in collections (`ISC004`) ([#&#8203;21972](https://redirect.github.com/astral-sh/ruff/pull/21972))
- \[`flake8-use-pathlib`] Make fixes unsafe when types change in compound statements (`PTH104`, `PTH105`, `PTH109`, `PTH115`) ([#&#8203;22009](https://redirect.github.com/astral-sh/ruff/pull/22009))
- \[`refurb`] Extend support for `Path.open` (`FURB101`, `FURB103`) ([#&#8203;21080](https://redirect.github.com/astral-sh/ruff/pull/21080))

##### Bug fixes

- \[`pyupgrade`] Fix parsing named Unicode escape sequences (`UP032`) ([#&#8203;21901](https://redirect.github.com/astral-sh/ruff/pull/21901))

##### Rule changes

- \[`eradicate`] Ignore `ruff:disable` and `ruff:enable` comments in `ERA001` ([#&#8203;22038](https://redirect.github.com/astral-sh/ruff/pull/22038))
- \[`flake8-pytest-style`] Allow `match` and `check` keyword arguments without an expected exception type (`PT010`) ([#&#8203;21964](https://redirect.github.com/astral-sh/ruff/pull/21964))
- \[syntax-errors] Annotated name cannot be global ([#&#8203;20868](https://redirect.github.com/astral-sh/ruff/pull/20868))

##### Documentation

- Add `uv` and `ty` to the Ruff README ([#&#8203;21996](https://redirect.github.com/astral-sh/ruff/pull/21996))
- Document known lambda formatting deviations from Black ([#&#8203;21954](https://redirect.github.com/astral-sh/ruff/pull/21954))
- Update `setup.md` ([#&#8203;22024](https://redirect.github.com/astral-sh/ruff/pull/22024))
- \[`flake8-bandit`] Fix broken link (`S704`) ([#&#8203;22039](https://redirect.github.com/astral-sh/ruff/pull/22039))

##### Other changes

- Fix playground Share button showing "Copied!" before clipboard copy completes ([#&#8203;21942](https://redirect.github.com/astral-sh/ruff/pull/21942))

##### Contributors

- [@&#8203;dylwil3](https://redirect.github.com/dylwil3)
- [@&#8203;charliecloudberry](https://redirect.github.com/charliecloudberry)
- [@&#8203;charliermarsh](https://redirect.github.com/charliermarsh)
- [@&#8203;chirizxc](https://redirect.github.com/chirizxc)
- [@&#8203;ntBre](https://redirect.github.com/ntBre)
- [@&#8203;zanieb](https://redirect.github.com/zanieb)
- [@&#8203;amyreese](https://redirect.github.com/amyreese)
- [@&#8203;hauntsaninja](https://redirect.github.com/hauntsaninja)
- [@&#8203;11happy](https://redirect.github.com/11happy)
- [@&#8203;mahiro72](https://redirect.github.com/mahiro72)
- [@&#8203;MichaReiser](https://redirect.github.com/MichaReiser)
- [@&#8203;phongddo](https://redirect.github.com/phongddo)
- [@&#8203;PeterJCLaw](https://redirect.github.com/PeterJCLaw)

### [`v0.14.9`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0149)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.14.8...0.14.9)

Released on 2025-12-11.

##### Preview features

- \[`ruff`] New `RUF100` diagnostics for unused range suppressions ([#&#8203;21783](https://redirect.github.com/astral-sh/ruff/pull/21783))
- \[`pylint`] Detect subclasses of builtin exceptions (`PLW0133`) ([#&#8203;21382](https://redirect.github.com/astral-sh/ruff/pull/21382))

##### Bug fixes

- Fix comment placement in lambda parameters ([#&#8203;21868](https://redirect.github.com/astral-sh/ruff/pull/21868))
- Skip over trivia tokens after re-lexing ([#&#8203;21895](https://redirect.github.com/astral-sh/ruff/pull/21895))
- \[`flake8-bandit`] Fix false positive when using non-standard `CSafeLoader` path (S506). ([#&#8203;21830](https://redirect.github.com/astral-sh/ruff/pull/21830))
- \[`flake8-bugbear`] Accept immutable slice default arguments (`B008`) ([#&#8203;21823](https://redirect.github.com/astral-sh/ruff/pull/21823))

##### Rule changes

- \[`pydocstyle`] Suppress `D417` for parameters with `Unpack` annotations ([#&#8203;21816](https://redirect.github.com/astral-sh/ruff/pull/21816))

##### Performance

- Use `memchr` for computing line indexes ([#&#8203;21838](https://redirect.github.com/astral-sh/ruff/pull/21838))

##### Documentation

- Document `*.pyw` is included by default in preview ([#&#8203;21885](https://redirect.github.com/astral-sh/ruff/pull/21885))
- Document range suppressions, reorganize suppression docs ([#&#8203;21884](https://redirect.github.com/astral-sh/ruff/pull/21884))
- Update mkdocs-material to 9.7.0 (Insiders now free) ([#&#8203;21797](https://redirect.github.com/astral-sh/ruff/pull/21797))

##### Contributors

- [@&#8203;Avasam](https://redirect.github.com/Avasam)
- [@&#8203;MichaReiser](https://redirect.github.com/MichaReiser)
- [@&#8203;charliermarsh](https://redirect.github.com/charliermarsh)
- [@&#8203;amyreese](https://redirect.github.com/amyreese)
- [@&#8203;phongddo](https://redirect.github.com/phongddo)
- [@&#8203;prakhar1144](https://redirect.github.com/prakhar1144)
- [@&#8203;mahiro72](https://redirect.github.com/mahiro72)
- [@&#8203;ntBre](https://redirect.github.com/ntBre)
- [@&#8203;LoicRiegel](https://redirect.github.com/LoicRiegel)

### [`v0.14.8`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#0148)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.14.7...0.14.8)

Released on 2025-12-04.

##### Preview features

- \[`flake8-bugbear`] Catch `yield` expressions within other statements (`B901`) ([#&#8203;21200](https://redirect.github.com/astral-sh/ruff/pull/21200))
- \[`flake8-use-pathlib`] Mark fixes unsafe for return type changes (`PTH104`, `PTH105`, `PTH109`, `PTH115`) ([#&#8203;21440](https://redirect.github.com/astral-sh/ruff/pull/21440))

##### Bug fixes

- Fix syntax error false positives for `await` outside functions ([#&#8203;21763](https://redirect.github.com/astral-sh/ruff/pull/21763))
- \[`flake8-simplify`] Fix truthiness assumption for non-iterable arguments in tuple/list/set calls (`SIM222`, `SIM223`) ([#&#8203;21479](https://redirect.github.com/astral-sh/ruff/pull/21479))

##### Documentation

- Suggest using `--output-file` option in GitLab integration ([#&#8203;21706](https://redirect.github.com/astral-sh/ruff/pull/21706))

##### Other changes

- \[syntax-error] Default type parameter followed by non-default type parameter ([#&#8203;21657](https://redirect.github.com/astral-sh/ruff/pull/21657))

##### Contributors

- [@&#8203;kieran-ryan](https://redirect.github.com/kieran-ryan)
- [@&#8203;11happy](https://redirect.github.com/11happy)
- [@&#8203;danparizher](https://redirect.github.com/danparizher)
- [@&#8203;ntBre](https://redirect.github.com/ntBre)

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

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi41OS4wIiwidXBkYXRlZEluVmVyIjoiNDIuNTkuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-12-29 16:12_

---

_Merged by @MichaReiser on 2025-12-29 16:23_

---

_Closed by @MichaReiser on 2025-12-29 16:23_

---

_Branch deleted on 2025-12-29 16:23_

---
