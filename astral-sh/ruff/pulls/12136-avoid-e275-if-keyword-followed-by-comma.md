```yaml
number: 12136
title: "Avoid `E275` if keyword followed by comma"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: dhruv/infinite-autofix
created_at: 2024-07-01T12:20:31Z
updated_at: 2024-07-01T12:34:25Z
url: https://github.com/astral-sh/ruff/pull/12136
synced_at: 2026-01-10T21:56:00Z
```

# Avoid `E275` if keyword followed by comma

---

_Pull request opened by @dhruvmanila on 2024-07-01 12:20_

## Summary

Use the following to reproduce this:
```console
$ cargo run -- check --select=E275,E203 --preview --no-cache ~/playground/ruff/src/play.py --fix
debug error: Failed to converge after 100 iterations in `/Users/dhruv/playground/ruff/src/play.py` with rule codes E275:---
yield,x

---
/Users/dhruv/playground/ruff/src/play.py:1:1: E275 Missing whitespace after keyword
  |
1 | yield,x
  | ^^^^^ E275
  |
  = help: Added missing whitespace after keyword

Found 101 errors (100 fixed, 1 remaining).
[*] 1 fixable with the `--fix` option.
```

## Test Plan

Add a test case and run `cargo insta test`.

---

_Label `bug` added by @dhruvmanila on 2024-07-01 12:20_

---

_Label `fuzzer` added by @dhruvmanila on 2024-07-01 12:20_

---

_@charliermarsh approved on 2024-07-01 12:21_

---

_Comment by @github-actions[bot] on 2024-07-01 12:33_

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

_Merged by @dhruvmanila on 2024-07-01 12:34_

---

_Closed by @dhruvmanila on 2024-07-01 12:34_

---

_Branch deleted on 2024-07-01 12:34_

---
