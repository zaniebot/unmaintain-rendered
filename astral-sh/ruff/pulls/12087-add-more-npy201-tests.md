```yaml
number: 12087
title: Add more NPY201 tests
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: npy201-tests
created_at: 2024-06-28T07:46:51Z
updated_at: 2024-06-28T07:59:56Z
url: https://github.com/astral-sh/ruff/pull/12087
synced_at: 2026-01-12T15:55:40Z
```

# Add more NPY201 tests

---

_@MichaReiser_

## Summary

Adds more tests for `NPY201` as mentioned [here](https://github.com/astral-sh/ruff/pull/12065#issuecomment-2196315803).

I also split the tests into multiple files and reduced the max iteration limit back to 10. 

## Test Plan

`cargo test`


---

_Comment by @MichaReiser on 2024-06-28 07:47_

@mtsokol could you take a look at the test output. Does the output look correct to you?

The output of the newly added tests are all in 

https://github.com/astral-sh/ruff/pull/12087/commits/0420ed1ab6d818db2ca12a8c6debfdf524f9cf8e

---

_Label `internal` added by @MichaReiser on 2024-06-28 07:47_

---

_@mtsokol reviewed on 2024-06-28 07:53_

Yes! They're all correct - thanks! 

---

_Merged by @MichaReiser on 2024-06-28 07:58_

---

_Closed by @MichaReiser on 2024-06-28 07:58_

---

_Branch deleted on 2024-06-28 07:58_

---

_Comment by @github-actions[bot] on 2024-06-28 07:59_

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
