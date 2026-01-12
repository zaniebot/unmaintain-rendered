```yaml
number: 17299
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
created_at: 2026-01-02T17:12:18Z
updated_at: 2026-01-02T17:51:51Z
url: https://github.com/astral-sh/uv/pull/17299
synced_at: 2026-01-12T16:12:42Z
```

# Update actions/setup-python action to v6

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [actions/setup-python](https://redirect.github.com/actions/setup-python) | action | major | `v5.6.0` → `v6.1.0` |

---

### Release Notes

<details>
<summary>actions/setup-python (actions/setup-python)</summary>

### [`v6.1.0`](https://redirect.github.com/actions/setup-python/releases/tag/v6.1.0)

[Compare Source](https://redirect.github.com/actions/setup-python/compare/v6.0.0...v6.1.0)

##### What's Changed

##### Enhancements:

- Add support for `pip-install` input by [@&#8203;gowridurgad](https://redirect.github.com/gowridurgad) in [#&#8203;1201](https://redirect.github.com/actions/setup-python/pull/1201)
- Add graalpy early-access and windows builds by [@&#8203;timfel](https://redirect.github.com/timfel) in [#&#8203;880](https://redirect.github.com/actions/setup-python/pull/880)

##### Dependency and Documentation updates:

- Enhanced wording and updated example usage for `allow-prereleases` by [@&#8203;yarikoptic](https://redirect.github.com/yarikoptic) in [#&#8203;979](https://redirect.github.com/actions/setup-python/pull/979)
- Upgrade urllib3 from 1.26.19 to 2.5.0 and document breaking changes in v6 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [#&#8203;1139](https://redirect.github.com/actions/setup-python/pull/1139)
- Upgrade typescript from 5.4.2 to 5.9.3 and Documentation update by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [#&#8203;1094](https://redirect.github.com/actions/setup-python/pull/1094)
- Upgrade actions/publish-action from 0.3.0 to 0.4.0 & Documentation update for pip-install input by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [#&#8203;1199](https://redirect.github.com/actions/setup-python/pull/1199)
- Upgrade requests from 2.32.2 to 2.32.4 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [#&#8203;1130](https://redirect.github.com/actions/setup-python/pull/1130)
- Upgrade prettier from 3.5.3 to 3.6.2 by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [#&#8203;1234](https://redirect.github.com/actions/setup-python/pull/1234)
- Upgrade [@&#8203;types/node](https://redirect.github.com/types/node) from 24.1.0 to 24.9.1 and update macos-13 to macos-15-intel by [@&#8203;dependabot](https://redirect.github.com/dependabot) in [#&#8203;1235](https://redirect.github.com/actions/setup-python/pull/1235)

##### New Contributors

- [@&#8203;yarikoptic](https://redirect.github.com/yarikoptic) made their first contribution in [#&#8203;979](https://redirect.github.com/actions/setup-python/pull/979)

**Full Changelog**: <https://github.com/actions/setup-python/compare/v6...v6.1.0>

### [`v6.0.0`](https://redirect.github.com/actions/setup-python/releases/tag/v6.0.0)

[Compare Source](https://redirect.github.com/actions/setup-python/compare/v5.6.0...v6)

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

📅 **Schedule**: Branch creation - Between 12:00 AM and 03:59 AM, only on Monday ( * 0-3 * * 1 ) (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi42OS4xIiwidXBkYXRlZEluVmVyIjoiNDIuNjkuMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2026-01-02 17:12_

---

_@woodruffw approved on 2026-01-02 17:51_

---

_Merged by @woodruffw on 2026-01-02 17:51_

---

_Closed by @woodruffw on 2026-01-02 17:51_

---

_Branch deleted on 2026-01-02 17:51_

---
