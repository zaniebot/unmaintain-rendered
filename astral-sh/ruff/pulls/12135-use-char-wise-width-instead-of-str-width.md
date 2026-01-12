```yaml
number: 12135
title: "Use char-wise width instead of `str`-width"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/char-wise-width
created_at: 2024-07-01T11:30:08Z
updated_at: 2024-07-01T13:29:27Z
url: https://github.com/astral-sh/ruff/pull/12135
synced_at: 2026-01-12T15:55:40Z
```

# Use char-wise width instead of `str`-width

---

_@dhruvmanila_

## Summary

This PR updates various references in the linter to compute the line-width for summing the width of each `char` in a `str` instead of computing the width of the `str` itself.

Refer to #12133 for more details.

fixes: #12130 

## Test Plan

Add a file with null (`\0`) character which is zero-width. Run this test case on `main` to make sure it panics and switch over to this branch to make sure it doesn't panic now.

---

_Comment by @github-actions[bot] on 2024-07-01 11:43_

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

_Label `bug` added by @dhruvmanila on 2024-07-01 12:24_

---

_Marked ready for review by @dhruvmanila on 2024-07-01 12:33_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-07-01 12:34_

---

_Comment by @codspeed-hq[bot] on 2024-07-01 12:36_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/char-wise-width)

### Merging #12135 will **not alter performance**

<sub>Comparing <code>dhruv/char-wise-width</code> (a8cd872) with <code>main</code> (37f260b)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E501_E501_4.py.snap`:1 on 2024-07-01 12:52_

Hmm, why is this considered to be a binary file?

---

_@MichaReiser reviewed on 2024-07-01 12:52_

---

_@MichaReiser reviewed on 2024-07-01 12:52_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/fix/snippet.rs`:42 on 2024-07-01 12:52_

I think we can keep using `width` here as it isn't related to any "overlong" logic.

---

_@MichaReiser approved on 2024-07-01 12:53_

---

_@dhruvmanila reviewed on 2024-07-01 12:54_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E501_E501_4.py.snap`:1 on 2024-07-01 12:54_

It contains `\0` character. I think I'll just inline the source code of that file as a separate test case to not do this.

---

_@dhruvmanila reviewed on 2024-07-01 13:14_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E501_E501_4.py.snap`:1 on 2024-07-01 13:14_

I think it's just the diff tab that renders it like that. The normal GitHub view is as: https://github.com/astral-sh/ruff/blob/dhruv/char-wise-width/crates/ruff_linter/resources/test/fixtures/pycodestyle/E501_4.py

---

_Merged by @dhruvmanila on 2024-07-01 13:26_

---

_Closed by @dhruvmanila on 2024-07-01 13:26_

---

_Branch deleted on 2024-07-01 13:26_

---
