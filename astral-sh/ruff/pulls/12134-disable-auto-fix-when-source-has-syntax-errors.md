```yaml
number: 12134
title: Disable auto-fix when source has syntax errors
type: pull_request
state: merged
author: dhruvmanila
labels:
  - fixes
assignees: []
merged: true
base: main
head: dhruv/disable-autofix
created_at: 2024-07-01T11:17:51Z
updated_at: 2024-07-02T08:52:53Z
url: https://github.com/astral-sh/ruff/pull/12134
synced_at: 2026-01-10T21:56:00Z
```

# Disable auto-fix when source has syntax errors

---

_Pull request opened by @dhruvmanila on 2024-07-01 11:17_

## Summary

This PR updates Ruff to **not** generate auto-fixes if the source code contains syntax errors as determined by the parser.

The main motivation behind this is to avoid infinite autofix loop when the token-based rules are run over any source with syntax errors in #11950.

Although even after this, it's not certain that there won't be an infinite autofix loop because the logic might be incorrect. For example, https://github.com/astral-sh/ruff/issues/12094 and https://github.com/astral-sh/ruff/pull/12136.

This requires updating the test infrastructure to not validate for fix availability status when the source contained syntax errors. This is required because otherwise the fuzzer might fail as it uses the test function to run the linter and validate the source code.

resolves: #11455 

## Test Plan

`cargo insta test`


---

_Comment by @github-actions[bot] on 2024-07-01 11:52_

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

_Marked ready for review by @dhruvmanila on 2024-07-01 14:39_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-07-01 14:39_

---

_@charliermarsh approved on 2024-07-01 22:48_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:296 on 2024-07-02 05:53_

I wasn't aware that we already had a similar loop. I guess this is okay. It does have the downside that we waste time on generating fixes that we then just omit. But I see why we do it this way, because we lack a better abstraction for testing if a fix should be generated.

---

_@MichaReiser approved on 2024-07-02 05:53_

---

_Label `fixes` added by @dhruvmanila on 2024-07-02 08:52_

---

_Merged by @dhruvmanila on 2024-07-02 08:52_

---

_Closed by @dhruvmanila on 2024-07-02 08:52_

---

_Branch deleted on 2024-07-02 08:52_

---
