```yaml
number: 19479
title: Run MD tests for Markdown-only changes
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
assignees: []
merged: true
base: main
head: david/run-mdtests-for-markdown-changes
created_at: 2025-07-22T08:00:55Z
updated_at: 2025-07-22T09:32:59Z
url: https://github.com/astral-sh/ruff/pull/19479
synced_at: 2026-01-10T17:58:13Z
```

# Run MD tests for Markdown-only changes

---

_Pull request opened by @sharkdp on 2025-07-22 08:00_

## Summary

Exclusions in Git pathspecs [are not order-sensitive](https://css-tricks.com/git-pathspecs-and-how-to-use-them/#aa-exclude):

> After all other pathspecs have been resolved, all pathspecs with an exclude signature are resolved and then removed from the returned paths.

This means that we can't write chains like we had here before to exclude Markdown file changes *unless* they are in `crates/ty_python_semantic/resources/mdtest`. This doesn't work. The exclude pattern will just overwrite the second pattern and all Markdown changes will be excluded:

```bash
':!**/*.md' \
':crates/ty_python_semantic/resources/mdtest/**/*.md' \
```

The configuration we had here before meant that tests wouldn't run on MD-test only PRs, see e.g. https://github.com/astral-sh/ruff/pull/19476.

So here, I'm proposing to remove the broad `:!**/*.md` pattern. We can always add more fine-grained exclusion patterns, if that's needed. The `docs` folder is already excluded.

## Test Plan

Tested with local `git diff` invocations.

---

_Label `ci` added by @sharkdp on 2025-07-22 08:00_

---

_@sharkdp reviewed on 2025-07-22 08:01_

---

_Review comment by @sharkdp on `.github/workflows/ci.yaml`:146 on 2025-07-22 08:01_

The `:**` inclusion pattern also seems unnecessary

---

_@sharkdp reviewed on 2025-07-22 08:01_

---

_Review comment by @sharkdp on `.github/workflows/ci.yaml`:151 on 2025-07-22 08:01_

YAML files are not excluded, so there's no need to include `ci.yaml` specifically.

---

_Comment by @github-actions[bot] on 2025-07-22 08:12_

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
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
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
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:150 on 2025-07-22 09:17_

could maybe add a comment here about why we exclude `docs/**` but not other `.md` files -- it'll be non-obvious to Ruff contributors who don't know anything about ty

---

_@AlexWaygood approved on 2025-07-22 09:17_

---

_Merged by @sharkdp on 2025-07-22 09:29_

---

_Closed by @sharkdp on 2025-07-22 09:29_

---

_Branch deleted on 2025-07-22 09:29_

---
