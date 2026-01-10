```yaml
number: 15857
title: Update astral-sh/setup-uv action to v6.7.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/astral-sh-setup-uv-6.x
created_at: 2025-09-15T01:18:54Z
updated_at: 2025-09-15T02:50:07Z
url: https://github.com/astral-sh/uv/pull/15857
synced_at: 2026-01-10T06:36:15Z
```

# Update astral-sh/setup-uv action to v6.7.0

---

_Pull request opened by @renovate on 2025-09-15 01:18_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [astral-sh/setup-uv](https://redirect.github.com/astral-sh/setup-uv) | action | minor | `v6.4.3` -> `v6.7.0` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/setup-uv (astral-sh/setup-uv)</summary>

### [`v6.7.0`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v6.7.0): 🌈 New inputs `restore-cache` and `save-cache`

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v6.6.1...v6.7.0)

#### Changes

This release adds fine-grained control over the caching steps.

- The input `restore-cache` (`true` by default) can be set to `false` to skip restoring the cache while still allowing to save the cache.
- The input `save-cache` (`true` by default) can be set to `false` to skip saving the cache.

Skipping cache saving can be useful if you know, that you will never use this version of the cache again and don't want to waste storage space:

```yaml
- name: Save cache only on main branch
  uses: astral-sh/setup-uv@v6
  with:
    enable-cache: true
    save-cache: ${{ github.ref == 'refs/heads/main' }}
```

#### 🚀 Enhancements

- Add inputs restore-cache and save-cache [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;568](https://redirect.github.com/astral-sh/setup-uv/issues/568))

#### 🧰 Maintenance

- bump deps [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;569](https://redirect.github.com/astral-sh/setup-uv/issues/569))
- Automatically push updated known versions [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;565](https://redirect.github.com/astral-sh/setup-uv/issues/565))
- chore: update known versions for 0.8.16/0.8.17 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;562](https://redirect.github.com/astral-sh/setup-uv/issues/562))
- chore: update known versions for 0.8.15 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;550](https://redirect.github.com/astral-sh/setup-uv/issues/550))
- chore(ci): address CI lint findings [@&#8203;woodruffw](https://redirect.github.com/woodruffw) ([#&#8203;545](https://redirect.github.com/astral-sh/setup-uv/issues/545))

#### ⬆️ Dependency updates

- Bump github/codeql-action from 3.29.11 to 3.30.3 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;566](https://redirect.github.com/astral-sh/setup-uv/issues/566))
- Bump actions/setup-node from 4.4.0 to 5.0.0 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;551](https://redirect.github.com/astral-sh/setup-uv/issues/551))

### [`v6.6.1`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v6.6.1): 🌈 Fix exclusions in cache-dependency-glob

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v6.6.0...v6.6.1)

##### Changes

Exclusions with a leading `!` in the [cache-dependency-glob](https://redirect.github.com/astral-sh/setup-uv?tab=readme-ov-file#cache-dependency-glob) did not work and got fixed with this release. Thank you [@&#8203;KnisterPeter](https://redirect.github.com/KnisterPeter) for raising this!

##### 🐛 Bug fixes

- Fix exclusions in cache-dependency-glob [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;546](https://redirect.github.com/astral-sh/setup-uv/issues/546))

##### 🧰 Maintenance

- Bump dependencies [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;547](https://redirect.github.com/astral-sh/setup-uv/issues/547))
- chore: update known versions for 0.8.14 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;543](https://redirect.github.com/astral-sh/setup-uv/issues/543))
- chore: update known versions for 0.8.13 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;536](https://redirect.github.com/astral-sh/setup-uv/issues/536))

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

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS45Ny4xMCIsInVwZGF0ZWRJblZlciI6IjQxLjk3LjEwIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-09-15 01:18_

---

_Merged by @charliermarsh on 2025-09-15 02:50_

---

_Closed by @charliermarsh on 2025-09-15 02:50_

---

_Branch deleted on 2025-09-15 02:50_

---
