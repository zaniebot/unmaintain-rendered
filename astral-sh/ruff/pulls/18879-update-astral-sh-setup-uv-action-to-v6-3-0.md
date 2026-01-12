```yaml
number: 18879
title: Update astral-sh/setup-uv action to v6.3.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/astral-sh-setup-uv-6.x
created_at: 2025-06-23T00:33:29Z
updated_at: 2025-06-23T06:03:08Z
url: https://github.com/astral-sh/ruff/pull/18879
synced_at: 2026-01-12T15:56:27Z
```

# Update astral-sh/setup-uv action to v6.3.0

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [astral-sh/setup-uv](https://redirect.github.com/astral-sh/setup-uv) | action | minor | `v6.1.0` -> `v6.3.0` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>astral-sh/setup-uv (astral-sh/setup-uv)</summary>

### [`v6.3.0`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v6.3.0): 🌈 Use latest version from manifest-file

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v6.2.1...v6.3.0)

#### Changes

If a manifest-file is supplied the default value of the version input (latest) will get the latest version available in the manifest. That might not be the actual latest version available in the official uv repo.

#### 🚀 Enhancements

- Use latest version from manifest-file [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;458](https://redirect.github.com/astral-sh/setup-uv/issues/458))

### [`v6.2.1`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v6.2.1): 🌈 Fix &quot;No such file or directory version-manifest.json&quot;

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v6.2.0...v6.2.1)

##### Changes

Release v6.2.0 contained a bug that slipped through the automated test. The action tried to look for the default version-manifest.json in the root of the repostory using this action instead of relative to the action itself.

##### 🐛 Bug fixes

- Look for version-manifest.json relative to action path [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;456](https://redirect.github.com/astral-sh/setup-uv/issues/456))

### [`v6.2.0`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v6.2.0): 🌈  New input manifest-file

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v6.1.0...v6.2.0)

##### Changes

This release adds a new input `manifest-file`.

The `manifest-file` input allows you to specify a JSON manifest that lists available uv versions,
architectures, and their download URLs. By default, this action uses the manifest file contained
in this repository, which is automatically updated with each release of uv.

The manifest file contains an array of objects, each describing a version,
architecture, platform, and the corresponding download URL.

You can supply a custom manifest file URL to define additional versions,
architectures, or different download URLs.
This is useful if you maintain your own uv builds or want to override the default sources.

For example:

```json
[
  {
    "version": "0.7.12-alpha.1",
    "artifactName": "uv-x86_64-unknown-linux-gnu.tar.gz",
    "arch": "x86_64",
    "platform": "unknown-linux-gnu",
    "downloadUrl": "https://release.pyx.dev/0.7.12-alpha.1/uv-x86_64-unknown-linux-gnu.tar.gz"
  },
  ...
]
```

```yaml
- name: Use a custom manifest file
  uses: astral-sh/setup-uv@v6
  with:
    manifest-file: "https://example.com/my-custom-manifest.json"
```

> \[!WARNING]\
> If you have previously used `server-url` to use your self hosted uv binaries use this new way instead.
> `server-url` is deprecated and will be removed in a future release

##### 🚀 Enhancements

- Add input manifest-file [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;454](https://redirect.github.com/astral-sh/setup-uv/issues/454))
- Add warning about shadowed uv binaries to `activate-environment` [@&#8203;zanieb](https://redirect.github.com/zanieb) ([#&#8203;439](https://redirect.github.com/astral-sh/setup-uv/issues/439))

##### 🧰 Maintenance

- chore: update known versions for 0.7.13 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;444](https://redirect.github.com/astral-sh/setup-uv/issues/444))
- Set expected cache dir drive to C: on windows [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;451](https://redirect.github.com/astral-sh/setup-uv/issues/451))
- chore: update known versions for 0.7.11 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;442](https://redirect.github.com/astral-sh/setup-uv/issues/442))
- chore: update known versions for 0.7.10 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;440](https://redirect.github.com/astral-sh/setup-uv/issues/440))
- chore: update known versions for 0.7.9 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;437](https://redirect.github.com/astral-sh/setup-uv/issues/437))
- Check that all jobs are in all-tests-passed.needs [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;432](https://redirect.github.com/astral-sh/setup-uv/issues/432))
- chore: update known versions for 0.7.8 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;428](https://redirect.github.com/astral-sh/setup-uv/issues/428))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MC42Mi4xIiwidXBkYXRlZEluVmVyIjoiNDAuNjIuMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-06-23 00:33_

---

_Comment by @github-actions[bot] on 2025-06-23 00:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-06-23 00:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+7 -0 violations, +0 -2 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/05994319b72d96facf065a4299b2ad9b79a25ab9/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L444'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:444:61:</a> PLC1802 [*] `len(SEQUENCE)` used as condition without comparison
+ <a href='https://github.com/apache/superset/blob/05994319b72d96facf065a4299b2ad9b79a25ab9/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L444'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:444:61:</a> PLC1802 `len(SEQUENCE)` used as condition without comparison
+ <a href='https://github.com/apache/superset/blob/05994319b72d96facf065a4299b2ad9b79a25ab9/superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py#L91'>superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py:91:20:</a> PLC1802 [*] `len(keys)` used as condition without comparison
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/1da0d022057862f4352113d884648606efd60099/pandas/core/frame.py#L7238'>pandas/core/frame.py:7238:14:</a> PLC1802 [*] `len(by)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/1da0d022057862f4352113d884648606efd60099/pandas/core/indexes/base.py#L7621'>pandas/core/indexes/base.py:7621:12:</a> PLC1802 [*] `len(index_like)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/1da0d022057862f4352113d884648606efd60099/pandas/io/parsers/base_parser.py#L246'>pandas/io/parsers/base_parser.py:246:12:</a> PLC1802 [*] `len(ic)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/1da0d022057862f4352113d884648606efd60099/pandas/io/parsers/python_parser.py#L284'>pandas/io/parsers/python_parser.py:284:16:</a> PLC1802 [*] `len(content)` used as condition without comparison
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/tests/runner.py#L547'>astropy/tests/runner.py:547:16:</a> PLC1802 [*] `len(paths)` used as condition without comparison
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/units/core.py#L1328'>astropy/units/core.py:1328:12:</a> PLC1802 [*] `len(results)` used as condition without comparison
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC1802 | 9 | 7 | 0 | 0 | 2 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+7 -0 violations, +0 -2 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/05994319b72d96facf065a4299b2ad9b79a25ab9/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L444'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:444:61:</a> PLC1802 [*] `len(SEQUENCE)` used as condition without comparison
+ <a href='https://github.com/apache/superset/blob/05994319b72d96facf065a4299b2ad9b79a25ab9/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L444'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:444:61:</a> PLC1802 `len(SEQUENCE)` used as condition without comparison
+ <a href='https://github.com/apache/superset/blob/05994319b72d96facf065a4299b2ad9b79a25ab9/superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py#L91'>superset/migrations/versions/2018-11-12_13-31_4ce8df208545_migrate_time_range_for_default_filters.py:91:20:</a> PLC1802 [*] `len(keys)` used as condition without comparison
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/1da0d022057862f4352113d884648606efd60099/pandas/core/frame.py#L7238'>pandas/core/frame.py:7238:14:</a> PLC1802 [*] `len(by)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/1da0d022057862f4352113d884648606efd60099/pandas/core/indexes/base.py#L7621'>pandas/core/indexes/base.py:7621:12:</a> PLC1802 [*] `len(index_like)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/1da0d022057862f4352113d884648606efd60099/pandas/io/parsers/base_parser.py#L246'>pandas/io/parsers/base_parser.py:246:12:</a> PLC1802 [*] `len(ic)` used as condition without comparison
+ <a href='https://github.com/pandas-dev/pandas/blob/1da0d022057862f4352113d884648606efd60099/pandas/io/parsers/python_parser.py#L284'>pandas/io/parsers/python_parser.py:284:16:</a> PLC1802 [*] `len(content)` used as condition without comparison
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/tests/runner.py#L547'>astropy/tests/runner.py:547:16:</a> PLC1802 [*] `len(paths)` used as condition without comparison
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/units/core.py#L1328'>astropy/units/core.py:1328:12:</a> PLC1802 [*] `len(results)` used as condition without comparison
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC1802 | 9 | 7 | 0 | 0 | 2 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @MichaReiser on 2025-06-23 06:03_

---

_Closed by @MichaReiser on 2025-06-23 06:03_

---

_Branch deleted on 2025-06-23 06:03_

---
