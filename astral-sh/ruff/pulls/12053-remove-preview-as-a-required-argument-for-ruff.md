```yaml
number: 12053
title: "Remove `--preview` as a required argument for `ruff server`"
type: pull_request
state: merged
author: snowsignal
labels:
  - internal
  - server
assignees: []
merged: true
base: main
head: jane/server/remove-preview-requirement
created_at: 2024-06-26T18:44:52Z
updated_at: 2024-07-05T05:57:15Z
url: https://github.com/astral-sh/ruff/pull/12053
synced_at: 2026-01-12T15:55:40Z
```

# Remove `--preview` as a required argument for `ruff server`

---

_@snowsignal_

## Summary

`ruff server` has reached a point of stabilization, and `--preview` is no longer required as a flag.

`--preview` is still supported as a flag, since future features may be need to gated behind it initially.

## Test Plan

A simple way to test this is to run `ruff server` from the command line. No error about a missing `--preview` argument should be reported.


---

_Label `server` added by @snowsignal on 2024-06-26 18:44_

---

_Added to milestone `Ruff Server: Stable` by @snowsignal on 2024-06-26 18:44_

---

_Review requested from @carljm by @snowsignal on 2024-06-26 18:44_

---

_Review requested from @MichaReiser by @snowsignal on 2024-06-26 18:44_

---

_Review requested from @dhruvmanila by @snowsignal on 2024-06-26 18:44_

---

_Comment by @github-actions[bot] on 2024-06-26 19:01_

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

_@MichaReiser approved on 2024-06-27 06:03_

I'm okay merging this but there are at least two follow-ups that are necessary

* Don't set the `--preview` flag in `ruff-vscode` if the `server` version is `>= 0.5`. We should do this pre-release to avoid that users will unintentionally opt in the future.
* I would expect that `ruff server --preview` works the same as `ruff check --preview` and `ruff format --preview` and enables all `--preview` features (linter and formatter). 

I would expect that running `ruff server --preview` would enable all preview features similar to setting `preview = true` in the settings. 

---

_Comment by @snowsignal on 2024-06-27 19:26_

@MichaReiser Thanks for the review! I'll look into addressing the points you brought up.

> I would expect that running `ruff server --preview` would enable all preview features similar to setting `preview = true` in the settings.

We also have server settings, `lint.preview`, and `format.preview`, that control this behavior. We should make it clear that this would override those settings.

---

_Merged by @snowsignal on 2024-06-27 19:27_

---

_Closed by @snowsignal on 2024-06-27 19:27_

---

_Branch deleted on 2024-06-27 19:27_

---

_Label `internal` added by @dhruvmanila on 2024-07-05 05:57_

---
