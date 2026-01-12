```yaml
number: 12072
title: Add necessary permissions for cargo-dist Docker build
type: pull_request
state: merged
author: charliermarsh
labels:
  - release
assignees: []
merged: true
base: main
head: charlie/docker
created_at: 2024-06-27T15:06:45Z
updated_at: 2024-06-27T15:25:13Z
url: https://github.com/astral-sh/ruff/pull/12072
synced_at: 2026-01-12T15:55:40Z
```

# Add necessary permissions for cargo-dist Docker build

---

_@charliermarsh_

## Summary

We have to do the same thing in uv: https://github.com/astral-sh/uv/blob/bf46792839e6d3dbb09d31aa497b9945b8f5151b/.github/workflows/release.yml#L103.

I think I undid this change locally while testing, it was definitely in the branch at one point!


---

_Label `release` added by @charliermarsh on 2024-06-27 15:06_

---

_@MichaReiser approved on 2024-06-27 15:07_

---

_@zanieb approved on 2024-06-27 15:12_

---

_Merged by @MichaReiser on 2024-06-27 15:16_

---

_Closed by @MichaReiser on 2024-06-27 15:16_

---

_Branch deleted on 2024-06-27 15:16_

---

_Comment by @github-actions[bot] on 2024-06-27 15:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `PLR1701` has been remapped to `SIM101`.
ruff failed
  Cause: Selection of deprecated rule `E999` is not allowed when preview is enabled.
```

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff format --preview --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `PLR1701` has been remapped to `SIM101`.
ruff failed
  Cause: Selection of deprecated rule `E999` is not allowed when preview is enabled.
```

</p>
</details>




---
