```yaml
number: 20408
title: Update actions/setup-python action to v6
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/actions-setup-python-6.x
created_at: 2025-09-15T02:55:53Z
updated_at: 2025-09-15T09:25:06Z
url: https://github.com/astral-sh/ruff/pull/20408
synced_at: 2026-01-10T17:40:28Z
```

# Update actions/setup-python action to v6

---

_Pull request opened by @renovate on 2025-09-15 02:55_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [actions/setup-python](https://redirect.github.com/actions/setup-python) | action | major | `v5.6.0` -> `v6.0.0` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>actions/setup-python (actions/setup-python)</summary>

### [`v6.0.0`](https://redirect.github.com/actions/setup-python/releases/tag/v6.0.0)

[Compare Source](https://redirect.github.com/actions/setup-python/compare/v5.6.0...v6.0.0)

#### What's Changed

##### Breaking Changes

- Upgrade to node 24 by [@&#8203;salmanmkc](https://redirect.github.com/salmanmkc) in [#&#8203;1164](https://redirect.github.com/actions/setup-python/pull/1164)

Make sure your runner is on version v2.327.1 or later to ensure compatibility with this release. [See Release Notes](https://redirect.github.com/actions/runner/releases/tag/v2.327.1)

##### Enhancements:

- Add support for `pip-version`  by [@&#8203;priyagupta108](https://redirect.github.com/priyagupta108) in [#&#8203;1129](https://redirect.github.com/actions/setup-python/pull/1129)
- Enhance reading from .python-version by [@&#8203;krystof-k](https://redirect.github.com/krystof-k) in [#&#8203;787](https://redirect.github.com/actions/setup-python/pull/787)
- Add version parsing from Pipfile by [@&#8203;aradkdj](https://redirect.github.com/aradkdj) in [#&#8203;1067](https://redirect.github.com/actions/setup-python/pull/1067)

##### Bug fixes:

- Clarify pythonLocation behaviour for PyPy and GraalPy in environment variables by [@&#8203;aparnajyothi-y](https://redirect.github.com/aparnajyothi-y) in [#&#8203;1183](https://redirect.github.com/actions/setup-python/pull/1183)
- Change missing cache directory error to warning  by [@&#8203;aparnajyothi-y](https://redirect.github.com/aparnajyothi-y) in [#&#8203;1182](https://redirect.github.com/actions/setup-python/pull/1182)
- Add Architecture-Specific PATH Management for Python with --user Flag on Windows by [@&#8203;aparnajyothi-y](https://redirect.github.com/aparnajyothi-y) in [#&#8203;1122](https://redirect.github.com/actions/setup-python/pull/1122)
- Include python version in PyPy python-version output by [@&#8203;cdce8p](https://redirect.github.com/cdce8p) in [#&#8203;1110](https://redirect.github.com/actions/setup-python/pull/1110)
- Update docs: clarification on pip authentication with setup-python by [@&#8203;priya-kinthali](https://redirect.github.com/priya-kinthali) in [#&#8203;1156](https://redirect.github.com/actions/setup-python/pull/1156)

##### Dependency updates:

- Upgrade idna from 2.9 to 3.7 in /**tests**/data by [@&#8203;dependabot](https://redirect.github.com/dependabot)\[bot] in [#&#8203;843](https://redirect.github.com/actions/setup-python/pull/843)
- Upgrade form-data to fix critical vulnerabilities [#&#8203;182](https://redirect.github.com/actions/setup-python/issues/182) & [#&#8203;183](https://redirect.github.com/actions/setup-python/issues/183) by [@&#8203;aparnajyothi-y](https://redirect.github.com/aparnajyothi-y) in [#&#8203;1163](https://redirect.github.com/actions/setup-python/pull/1163)
- Upgrade setuptools to 78.1.1 to fix path traversal vulnerability in PackageIndex.download by [@&#8203;aparnajyothi-y](https://redirect.github.com/aparnajyothi-y) in [#&#8203;1165](https://redirect.github.com/actions/setup-python/pull/1165)
- Upgrade actions/checkout from 4 to 5 by [@&#8203;dependabot](https://redirect.github.com/dependabot)\[bot] in [#&#8203;1181](https://redirect.github.com/actions/setup-python/pull/1181)
- Upgrade [@&#8203;actions/tool-cache](https://redirect.github.com/actions/tool-cache) from 2.0.1 to 2.0.2 by [@&#8203;dependabot](https://redirect.github.com/dependabot)\[bot] in [#&#8203;1095](https://redirect.github.com/actions/setup-python/pull/1095)

#### New Contributors

- [@&#8203;krystof-k](https://redirect.github.com/krystof-k) made their first contribution in [#&#8203;787](https://redirect.github.com/actions/setup-python/pull/787)
- [@&#8203;cdce8p](https://redirect.github.com/cdce8p) made their first contribution in [#&#8203;1110](https://redirect.github.com/actions/setup-python/pull/1110)
- [@&#8203;aradkdj](https://redirect.github.com/aradkdj) made their first contribution in [#&#8203;1067](https://redirect.github.com/actions/setup-python/pull/1067)

**Full Changelog**: <https://github.com/actions/setup-python/compare/v5...v6.0.0>

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS45Ny4xMCIsInVwZGF0ZWRJblZlciI6IjQxLjk3LjEwIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-09-15 02:55_

---

_Comment by @github-actions[bot] on 2025-09-15 03:25_

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

_Merged by @MichaReiser on 2025-09-15 09:25_

---

_Closed by @MichaReiser on 2025-09-15 09:25_

---

_Branch deleted on 2025-09-15 09:25_

---
