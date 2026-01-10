```yaml
number: 16557
title: Update dependency astral-sh/uv to v0.9.7
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/astral-sh-uv-0.x
created_at: 2025-11-03T00:27:53Z
updated_at: 2025-11-03T01:04:46Z
url: https://github.com/astral-sh/uv/pull/16557
synced_at: 2026-01-10T06:28:12Z
```

# Update dependency astral-sh/uv to v0.9.7

---

_Pull request opened by @renovate on 2025-11-03 00:27_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [astral-sh/uv](https://redirect.github.com/astral-sh/uv) | uses-with | patch | `0.9.5` -> `0.9.7` |

---

### Release Notes

<details>
<summary>astral-sh/uv (astral-sh/uv)</summary>

### [`v0.9.7`](https://redirect.github.com/astral-sh/uv/blob/HEAD/CHANGELOG.md#097)

[Compare Source](https://redirect.github.com/astral-sh/uv/compare/0.9.6...0.9.7)

Released on 2025-10-30.

##### Enhancements

- Add Windows x86-32 emulation support to interpreter architecture checks ([#&#8203;13475](https://redirect.github.com/astral-sh/uv/pull/13475))
- Improve readability of progress bars ([#&#8203;16509](https://redirect.github.com/astral-sh/uv/pull/16509))
- Add GitHub attestations for uv release artifacts ([#&#8203;11357](https://redirect.github.com/astral-sh/uv/pull/11357))

##### Bug fixes

- Drop terminal coloring from `uv auth token` output ([#&#8203;16504](https://redirect.github.com/astral-sh/uv/pull/16504))
- Don't use UV\_LOCKED to enable `--check` flag ([#&#8203;16521](https://redirect.github.com/astral-sh/uv/pull/16521))

### [`v0.9.6`](https://redirect.github.com/astral-sh/uv/blob/HEAD/CHANGELOG.md#096)

[Compare Source](https://redirect.github.com/astral-sh/uv/compare/0.9.5...0.9.6)

Released on 2025-10-29.

This release contains an upgrade to Astral's fork of `async_zip`, which addresses potential sources of ZIP parsing differentials between uv and other Python packaging tooling. See [GHSA-pqhf-p39g-3x64](https://redirect.github.com/astral-sh/uv/security/advisories/GHSA-pqhf-p39g-3x64) for additional details.

##### Security

- Address ZIP parsing differentials ([GHSA-pqhf-p39g-3x64](https://redirect.github.com/astral-sh/uv/security/advisories/GHSA-pqhf-p39g-3x64))

##### Python

- Upgrade GraalPy to 25.0.1 ([#&#8203;16401](https://redirect.github.com/astral-sh/uv/pull/16401))

##### Enhancements

- Add `--clear` to `uv build` to remove old build artifacts ([#&#8203;16371](https://redirect.github.com/astral-sh/uv/pull/16371))
- Add `--no-create-gitignore` to `uv build` ([#&#8203;16369](https://redirect.github.com/astral-sh/uv/pull/16369))
- Do not error when a virtual environment directory cannot be removed due to a busy error ([#&#8203;16394](https://redirect.github.com/astral-sh/uv/pull/16394))
- Improve hint on `pip install --system` when externally managed ([#&#8203;16392](https://redirect.github.com/astral-sh/uv/pull/16392))
- Running `uv lock --check` with outdated lockfile will print that `--check` was passed, instead of `--locked`  ([#&#8203;16322](https://redirect.github.com/astral-sh/uv/pull/16322))
- Update `uv init` template for Maturin ([#&#8203;16449](https://redirect.github.com/astral-sh/uv/pull/16449))
- Improve ordering of Python sources in logs ([#&#8203;16463](https://redirect.github.com/astral-sh/uv/pull/16463))
- Restore DockerHub release images and annotations ([#&#8203;16441](https://redirect.github.com/astral-sh/uv/pull/16441))

##### Bug fixes

- Check for matching Python implementation during `uv python upgrade` ([#&#8203;16420](https://redirect.github.com/astral-sh/uv/pull/16420))
- Deterministically order `--find-links` distributions ([#&#8203;16446](https://redirect.github.com/astral-sh/uv/pull/16446))
- Don't panic in `uv export --frozen` when the lockfile is outdated ([#&#8203;16407](https://redirect.github.com/astral-sh/uv/pull/16407))
- Fix root of `uv tree` when `--package` is used with circular dependencies ([#&#8203;15908](https://redirect.github.com/astral-sh/uv/pull/15908))
- Show package list with `pip freeze --quiet` ([#&#8203;16491](https://redirect.github.com/astral-sh/uv/pull/16491))
- Limit `uv auth login pyx.dev` retries to 60s ([#&#8203;16498](https://redirect.github.com/astral-sh/uv/pull/16498))
- Add an empty group with `uv add --group ... -r ...` ([#&#8203;16490](https://redirect.github.com/astral-sh/uv/pull/16490))

##### Documentation

- Update docs for maturin build backend init template ([#&#8203;16469](https://redirect.github.com/astral-sh/uv/pull/16469))
- Update docs to reflect previous changes to signal forwarding semantics ([#&#8203;16430](https://redirect.github.com/astral-sh/uv/pull/16430))
- Add instructions for installing via MacPorts ([#&#8203;16039](https://redirect.github.com/astral-sh/uv/pull/16039))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS4xNTkuNCIsInVwZGF0ZWRJblZlciI6IjQxLjE1OS40IiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-11-03 00:27_

---

_Merged by @charliermarsh on 2025-11-03 01:04_

---

_Closed by @charliermarsh on 2025-11-03 01:04_

---

_Branch deleted on 2025-11-03 01:04_

---
