```yaml
number: 12079
title: "Publish docs and playground on `cargo-dist` release"
type: pull_request
state: merged
author: charliermarsh
labels:
  - release
assignees: []
merged: true
base: main
head: charlie/publish
created_at: 2024-06-28T00:44:44Z
updated_at: 2024-06-28T11:29:06Z
url: https://github.com/astral-sh/ruff/pull/12079
synced_at: 2026-01-10T21:56:00Z
```

# Publish docs and playground on `cargo-dist` release

---

_Pull request opened by @charliermarsh on 2024-06-28 00:44_

## Summary

These are now `post-announce-jobs`. So if they fail, the release itself will still succeed, which seems ok. (If we make them `publish-jobs`, then we might end up publishing to PyPI but failing the release itself if one of these fails.)

The intent is that these are still runnable via `workflow_dispatch` too.

Closes https://github.com/astral-sh/ruff/issues/12074.

---

_Label `release` added by @charliermarsh on 2024-06-28 00:45_

---

_Review requested from @zanieb by @charliermarsh on 2024-06-28 00:45_

---

_Review requested from @konstin by @charliermarsh on 2024-06-28 00:45_

---

_Comment by @github-actions[bot] on 2024-06-28 01:03_

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

_@MichaReiser approved on 2024-06-28 07:09_

---

_@konstin approved on 2024-06-28 07:23_

---

_Merged by @charliermarsh on 2024-06-28 11:29_

---

_Closed by @charliermarsh on 2024-06-28 11:29_

---

_Branch deleted on 2024-06-28 11:29_

---
