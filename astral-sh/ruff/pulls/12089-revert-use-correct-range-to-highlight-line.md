```yaml
number: 12089
title: "Revert \"Use correct range to highlight line continuation error\""
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: main
head: dhruv/revert
created_at: 2024-06-28T11:26:21Z
updated_at: 2024-06-28T12:40:02Z
url: https://github.com/astral-sh/ruff/pull/12089
synced_at: 2026-01-12T15:55:40Z
```

# Revert "Use correct range to highlight line continuation error"

---

_@dhruvmanila_

This PR reverts https://github.com/astral-sh/ruff/pull/12016 with a small change where the error location points to the continuation character only. Earlier, it would also highlight the whitespace that came before it.

The motivation for this change is to avoid panic in https://github.com/astral-sh/ruff/pull/11950. For example:

```py
\)
```

Playground: https://play.ruff.rs/87711071-1b54-45a3-b45a-81a336a1ea61

The range of `Unknown` token and `Rpar` is the same. Once #11950 is enabled, the indexer would panic. It won't panic in the stable version because we stop at the first `Unknown` token.

---

_Label `parser` added by @dhruvmanila on 2024-06-28 11:26_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-28 11:26_

---

_Comment by @github-actions[bot] on 2024-06-28 11:44_

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

_@MichaReiser approved on 2024-06-28 11:52_

---

_Merged by @dhruvmanila on 2024-06-28 12:40_

---

_Closed by @dhruvmanila on 2024-06-28 12:40_

---

_Branch deleted on 2024-06-28 12:40_

---
