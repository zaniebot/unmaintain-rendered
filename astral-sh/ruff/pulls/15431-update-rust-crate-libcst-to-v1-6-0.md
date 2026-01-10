```yaml
number: 15431
title: Update Rust crate libcst to v1.6.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/libcst-1.x-lockfile
created_at: 2025-01-11T16:14:51Z
updated_at: 2025-01-11T17:19:18Z
url: https://github.com/astral-sh/ruff/pull/15431
synced_at: 2026-01-10T20:34:00Z
```

# Update Rust crate libcst to v1.6.0

---

_Pull request opened by @renovate on 2025-01-11 16:14_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [libcst](https://redirect.github.com/Instagram/LibCST) | workspace.dependencies | minor | `1.5.1` -> `1.6.0` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>Instagram/LibCST (libcst)</summary>

### [`v1.6.0`](https://redirect.github.com/Instagram/LibCST/blob/HEAD/CHANGELOG.md#160---2024-01-09)

[Compare Source](https://redirect.github.com/Instagram/LibCST/compare/v1.5.1...v1.6.0)

#### Fixed

-   rename: store state in scratch by [@&#8203;zsol](https://redirect.github.com/zsol) in [https://github.com/Instagram/LibCST/pull/1250](https://redirect.github.com/Instagram/LibCST/pull/1250)
-   rename: handle imports via a parent module by [@&#8203;zsol](https://redirect.github.com/zsol) in [https://github.com/Instagram/LibCST/pull/1251](https://redirect.github.com/Instagram/LibCST/pull/1251)
-   rename: Fix imports with aliases by [@&#8203;zsol](https://redirect.github.com/zsol) in [https://github.com/Instagram/LibCST/pull/1252](https://redirect.github.com/Instagram/LibCST/pull/1252)
-   rename: don't leave trailing commas by [@&#8203;zsol](https://redirect.github.com/zsol) in [https://github.com/Instagram/LibCST/pull/1254](https://redirect.github.com/Instagram/LibCST/pull/1254)
-   rename: don't eat commas unnecessarily by [@&#8203;zsol](https://redirect.github.com/zsol) in [https://github.com/Instagram/LibCST/pull/1256](https://redirect.github.com/Instagram/LibCST/pull/1256)
-   rename: fix renaming toplevel names by [@&#8203;zsol](https://redirect.github.com/zsol) in [https://github.com/Instagram/LibCST/pull/1260](https://redirect.github.com/Instagram/LibCST/pull/1260)
-   bump 3.12 to 3.13 in readme by [@&#8203;khameeteman](https://redirect.github.com/khameeteman) in [https://github.com/Instagram/LibCST/pull/1228](https://redirect.github.com/Instagram/LibCST/pull/1228)

#### Added

-   Add codemod to convert `typing.Union` to `|` by [@&#8203;yangdanny97](https://redirect.github.com/yangdanny97) in [https://github.com/Instagram/LibCST/pull/1270](https://redirect.github.com/Instagram/LibCST/pull/1270)
-   Add codemod to fix variadic callable annotations by [@&#8203;yangdanny97](https://redirect.github.com/yangdanny97) in [https://github.com/Instagram/LibCST/pull/1269](https://redirect.github.com/Instagram/LibCST/pull/1269)
-   Add codemod to rename typing aliases of builtins by [@&#8203;yangdanny97](https://redirect.github.com/yangdanny97) in [https://github.com/Instagram/LibCST/pull/1267](https://redirect.github.com/Instagram/LibCST/pull/1267)
-   Add typing classifier to pyproject.toml and badge to README by [@&#8203;yangdanny97](https://redirect.github.com/yangdanny97) in [https://github.com/Instagram/LibCST/pull/1272](https://redirect.github.com/Instagram/LibCST/pull/1272)
-   Expose TypeAlias and TypeVar related structs in rust library by [@&#8203;Crozzers](https://redirect.github.com/Crozzers) in [https://github.com/Instagram/LibCST/pull/1274](https://redirect.github.com/Instagram/LibCST/pull/1274)

#### Updated

-   Upgrade pyo3 to 0.22 by [@&#8203;jelmer](https://redirect.github.com/jelmer) in [https://github.com/Instagram/LibCST/pull/1180](https://redirect.github.com/Instagram/LibCST/pull/1180)

#### New Contributors

-   [@&#8203;yangdanny97](https://redirect.github.com/yangdanny97) made their first contribution in [https://github.com/Instagram/LibCST/pull/1270](https://redirect.github.com/Instagram/LibCST/pull/1270)
-   [@&#8203;Crozzers](https://redirect.github.com/Crozzers) made their first contribution in [https://github.com/Instagram/LibCST/pull/1274](https://redirect.github.com/Instagram/LibCST/pull/1274)
-   [@&#8203;jelmer](https://redirect.github.com/jelmer) made their first contribution in [https://github.com/Instagram/LibCST/pull/1180](https://redirect.github.com/Instagram/LibCST/pull/1180)

**Full Changelog**: https://github.com/Instagram/LibCST/compare/v1.5.1...v1.6.0

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS45Mi4wIiwidXBkYXRlZEluVmVyIjoiMzkuOTIuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-01-11 16:14_

---

_Merged by @MichaReiser on 2025-01-11 17:14_

---

_Closed by @MichaReiser on 2025-01-11 17:14_

---

_Branch deleted on 2025-01-11 17:14_

---

_Comment by @github-actions[bot] on 2025-01-11 17:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>




---
