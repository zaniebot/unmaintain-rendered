```yaml
number: 20074
title: Update astral-sh/setup-uv action to v6.6.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/astral-sh-setup-uv-6.x
created_at: 2025-08-25T02:23:23Z
updated_at: 2025-08-25T05:10:48Z
url: https://github.com/astral-sh/ruff/pull/20074
synced_at: 2026-01-10T17:46:21Z
```

# Update astral-sh/setup-uv action to v6.6.0

---

_Pull request opened by @renovate on 2025-08-25 02:23_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [astral-sh/setup-uv](https://redirect.github.com/astral-sh/setup-uv) | action | minor | `v6.4.3` -> `v6.6.0` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/setup-uv (astral-sh/setup-uv)</summary>

### [`v6.6.0`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v6.6.0): 🌈 Support for .tools-versions

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v6.5.0...v6.6.0)

##### Changes

This release adds support for [asdf](https://asdf-vm.com/) `.tool-versions` in the `version-file` input

##### 🐛 Bug fixes

- Add log message before long API calls to GitHub [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;530](https://redirect.github.com/astral-sh/setup-uv/issues/530))

##### 🚀 Enhancements

- Add support for .tools-versions [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;531](https://redirect.github.com/astral-sh/setup-uv/issues/531))

##### 🧰 Maintenance

- Bump dependencies [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;532](https://redirect.github.com/astral-sh/setup-uv/issues/532))
- chore: update known versions for 0.8.12 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;529](https://redirect.github.com/astral-sh/setup-uv/issues/529))
- chore: update known versions for 0.8.11 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;526](https://redirect.github.com/astral-sh/setup-uv/issues/526))
- chore: update known versions for 0.8.10 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;525](https://redirect.github.com/astral-sh/setup-uv/issues/525))

### [`v6.5.0`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v6.5.0): 🌈 Better error messages, bug fixes and copilot agent settings

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v6.4.3...v6.5.0)

##### Changes

This release brings better error messages in case the GitHub API is impacted, fixes a few bugs and allows to disable [problem matchers](https://redirect.github.com/actions/toolkit/blob/main/docs/problem-matchers.md) for better use in Copilot Agent workspaces.

##### 🐛 Bug fixes

- Improve error messages on GitHub API errors [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;518](https://redirect.github.com/astral-sh/setup-uv/issues/518))
- Ignore backslashes and whitespace in requirements [@&#8203;axm2](https://redirect.github.com/axm2) ([#&#8203;501](https://redirect.github.com/astral-sh/setup-uv/issues/501))

##### 🚀 Enhancements

- Add input add-problem-matchers [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;517](https://redirect.github.com/astral-sh/setup-uv/issues/517))

##### 🧰 Maintenance

- chore: update known versions for 0.8.9 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;512](https://redirect.github.com/astral-sh/setup-uv/issues/512))
- chore: update known versions for 0.8.6-0.8.8 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;510](https://redirect.github.com/astral-sh/setup-uv/issues/510))
- chore: update known versions for 0.8.5 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;509](https://redirect.github.com/astral-sh/setup-uv/issues/509))
- chore: update known versions for 0.8.4 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;505](https://redirect.github.com/astral-sh/setup-uv/issues/505))
- chore: update known versions for 0.8.3 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;502](https://redirect.github.com/astral-sh/setup-uv/issues/502))

##### 📚 Documentation

- add note on caching to read disable-cache-pruning [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;506](https://redirect.github.com/astral-sh/setup-uv/issues/506))

##### ⬆️ Dependency updates

- Bump actions/checkout from 4 to 5 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;514](https://redirect.github.com/astral-sh/setup-uv/issues/514))
- bump dependencies [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;516](https://redirect.github.com/astral-sh/setup-uv/issues/516))
- Bump biome to v2 [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;515](https://redirect.github.com/astral-sh/setup-uv/issues/515))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS44Mi43IiwidXBkYXRlZEluVmVyIjoiNDEuODIuNyIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-08-25 02:23_

---

_Comment by @github-actions[bot] on 2025-08-25 02:27_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-08-25 03:38_

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

_Merged by @dhruvmanila on 2025-08-25 05:10_

---

_Closed by @dhruvmanila on 2025-08-25 05:10_

---

_Branch deleted on 2025-08-25 05:10_

---
