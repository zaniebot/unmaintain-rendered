```yaml
number: 13847
title: Update dependency ruff to v0.7.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/ruff-0.x
created_at: 2024-10-21T01:42:25Z
updated_at: 2024-10-21T01:48:01Z
url: https://github.com/astral-sh/ruff/pull/13847
synced_at: 2026-01-10T20:59:37Z
```

# Update dependency ruff to v0.7.0

---

_Pull request opened by @renovate on 2024-10-21 01:42_

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [ruff](https://docs.astral.sh/ruff) ([source](https://redirect.github.com/astral-sh/ruff), [changelog](https://redirect.github.com/astral-sh/ruff/blob/main/CHANGELOG.md)) | `==0.6.9` -> `==0.7.0` | [![age](https://developer.mend.io/api/mc/badges/age/pypi/ruff/0.7.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/pypi/ruff/0.7.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/pypi/ruff/0.6.9/0.7.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/pypi/ruff/0.6.9/0.7.0?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

### Release Notes

<details>
<summary>astral-sh/ruff (ruff)</summary>

### [`v0.7.0`](https://redirect.github.com/astral-sh/ruff/blob/HEAD/CHANGELOG.md#070)

[Compare Source](https://redirect.github.com/astral-sh/ruff/compare/0.6.9...0.7.0)

Check out the [blog post](https://astral.sh/blog/ruff-v0.7.0) for a migration guide and overview of the changes!

##### Breaking changes

-   The pytest rules `PT001` and `PT023` now default to omitting the decorator parentheses when there are no arguments
    ([#&#8203;12838](https://redirect.github.com/astral-sh/ruff/pull/12838), [#&#8203;13292](https://redirect.github.com/astral-sh/ruff/pull/13292)).
    This was a change that we attempted to make in Ruff v0.6.0, but only partially made due to an error on our part.
    See the [blog post](https://astral.sh/blog/ruff-v0.7.0) for more details.
-   The `useless-try-except` rule (in our `tryceratops` category) has been recoded from `TRY302` to
    `TRY203` ([#&#8203;13502](https://redirect.github.com/astral-sh/ruff/pull/13502)). This ensures Ruff's code is consistent with
    the same rule in the [`tryceratops`](https://redirect.github.com/guilatrova/tryceratops) linter.
-   The `lint.allow-unused-imports` setting has been removed ([#&#8203;13677](https://redirect.github.com/astral-sh/ruff/pull/13677)). Use
    [`lint.pyflakes.allow-unused-imports`](https://docs.astral.sh/ruff/settings/#lint_pyflakes_allowed-unused-imports)
    instead.

##### Formatter preview style

-   Normalize implicit concatenated f-string quotes per part ([#&#8203;13539](https://redirect.github.com/astral-sh/ruff/pull/13539))

##### Preview linter features

-   \[`refurb`] implement `hardcoded-string-charset` (FURB156) ([#&#8203;13530](https://redirect.github.com/astral-sh/ruff/pull/13530))
-   \[`refurb`] Count codepoints not bytes for `slice-to-remove-prefix-or-suffix (FURB188)` ([#&#8203;13631](https://redirect.github.com/astral-sh/ruff/pull/13631))

##### Rule changes

-   \[`pylint`] Mark `PLE1141` fix as unsafe ([#&#8203;13629](https://redirect.github.com/astral-sh/ruff/pull/13629))
-   \[`flake8-async`] Consider async generators to be "checkpoints" for `cancel-scope-no-checkpoint` (`ASYNC100`) ([#&#8203;13639](https://redirect.github.com/astral-sh/ruff/pull/13639))
-   \[`flake8-bugbear`] Do not suggest setting parameter `strict=` to `False` in `B905` diagnostic message ([#&#8203;13656](https://redirect.github.com/astral-sh/ruff/pull/13656))
-   \[`flake8-todos`] Only flag the word "TODO", not words starting with "todo" (`TD006`) ([#&#8203;13640](https://redirect.github.com/astral-sh/ruff/pull/13640))
-   \[`pycodestyle`] Fix whitespace-related false positives and false negatives inside type-parameter lists (`E231`, `E251`) ([#&#8203;13704](https://redirect.github.com/astral-sh/ruff/pull/13704))
-   \[`flake8-simplify`] Stabilize preview behavior for `SIM115` so that the rule can detect files
    being opened from a wider range of standard-library functions ([#&#8203;12959](https://redirect.github.com/astral-sh/ruff/pull/12959)).

##### CLI

-   Add explanation of fixable in `--statistics` command ([#&#8203;13774](https://redirect.github.com/astral-sh/ruff/pull/13774))

##### Bug fixes

-   \[`pyflakes`] Allow `ipytest` cell magic (`F401`) ([#&#8203;13745](https://redirect.github.com/astral-sh/ruff/pull/13745))
-   \[`flake8-use-pathlib`] Fix `PTH123` false positive when `open` is passed a file descriptor ([#&#8203;13616](https://redirect.github.com/astral-sh/ruff/pull/13616))
-   \[`flake8-bandit`] Detect patterns from multi line SQL statements (`S608`) ([#&#8203;13574](https://redirect.github.com/astral-sh/ruff/pull/13574))
-   \[`flake8-pyi`] - Fix dropped expressions in `PYI030` autofix ([#&#8203;13727](https://redirect.github.com/astral-sh/ruff/pull/13727))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOC4xMjAuMSIsInVwZGF0ZWRJblZlciI6IjM4LjEyMC4xIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2024-10-21 01:42_

---

_Merged by @charliermarsh on 2024-10-21 01:48_

---

_Closed by @charliermarsh on 2024-10-21 01:48_

---

_Branch deleted on 2024-10-21 01:48_

---
