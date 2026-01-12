```yaml
number: 12059
title: Add server config to filter out syntax error diagnostics
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: ruff-0.5
head: dhruv/syntax-error-5
created_at: 2024-06-27T03:05:49Z
updated_at: 2024-06-27T07:04:53Z
url: https://github.com/astral-sh/ruff/pull/12059
synced_at: 2026-01-12T15:55:40Z
```

# Add server config to filter out syntax error diagnostics

---

_@dhruvmanila_

## Summary

Follow-up from #11901 

This PR adds a new server setting to show / hide syntax errors.

## Test Plan

### VS Code

Using https://github.com/astral-sh/ruff-vscode/pull/504 with the following config:

```json
{
  "ruff.nativeServer": true,
  "ruff.path": ["/Users/dhruv/work/astral/ruff/target/debug/ruff"],
  "ruff.showSyntaxErrors": true
}
```

First, set `ruff.showSyntaxErrors` to `true`:
<img width="1177" alt="Screenshot 2024-06-27 at 08 34 58" src="https://github.com/astral-sh/ruff/assets/67177269/5d77547a-a908-4a00-8714-7c00784e8679">

And then set it to `false`:
<img width="1185" alt="Screenshot 2024-06-27 at 08 35 19" src="https://github.com/astral-sh/ruff/assets/67177269/9720f089-f10c-420b-a2c1-2bbb2245be35">

### Neovim

Using the following Ruff server config:

```lua
require('lspconfig').ruff.setup {
  init_options = {
    settings = {
      showSyntaxErrors = false,
    },
  },
}
```

First, set `showSyntaxErrors` to `true`:
<img width="1279" alt="Screenshot 2024-06-27 at 08 28 03" src="https://github.com/astral-sh/ruff/assets/67177269/e694e231-91ba-47f8-8e8a-ad2e82b85a45">

And then set it to `false`:
<img width="1284" alt="Screenshot 2024-06-27 at 08 28 20" src="https://github.com/astral-sh/ruff/assets/67177269/25b86a57-02b1-44f7-9f65-cf5fdde93b0c">


---

_Review requested from @snowsignal by @dhruvmanila on 2024-06-27 03:05_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-27 03:06_

---

_@snowsignal approved on 2024-06-27 04:30_

---

_Label `server` added by @MichaReiser on 2024-06-27 06:28_

---

_@MichaReiser approved on 2024-06-27 06:29_

---

_Comment by @MichaReiser on 2024-06-27 06:29_

It's so much easier to think about backward compatibility with the new LSP where it isn't a combination of VS code extension version, LSP and ruff versions.

---

_@dhruvmanila reviewed on 2024-06-27 06:47_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/lint.rs`:191 on 2024-06-27 06:47_

This seemed like a quick win to avoid cloning `Url` via `make_key` for every diagnostics.

---

_Merged by @dhruvmanila on 2024-06-27 06:59_

---

_Closed by @dhruvmanila on 2024-06-27 06:59_

---

_Branch deleted on 2024-06-27 06:59_

---

_Comment by @github-actions[bot] on 2024-06-27 07:04_

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




---
