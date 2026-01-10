```yaml
number: 14814
title: Minor nitpicks for PTH210
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: ruf049-nits
created_at: 2024-12-06T12:18:36Z
updated_at: 2024-12-06T12:48:02Z
url: https://github.com/astral-sh/ruff/pull/14814
synced_at: 2026-01-10T20:42:27Z
```

# Minor nitpicks for PTH210

---

_Pull request opened by @AlexWaygood on 2024-12-06 12:18_

## Summary

Some fairly inconsequential nitpicks I had locally and was about to push before @MichaReiser merged #14779 😆

## Test Plan

`cargo test -p ruff_linter --lib`


---

_Label `internal` added by @AlexWaygood on 2024-12-06 12:18_

---

_@MichaReiser approved on 2024-12-06 12:20_

You got to be fast

---

_Merged by @AlexWaygood on 2024-12-06 12:23_

---

_Closed by @AlexWaygood on 2024-12-06 12:23_

---

_Branch deleted on 2024-12-06 12:23_

---

_Comment by @github-actions[bot] on 2024-12-06 12:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/console/application.py#L12'>src/poetry/console/application.py:12:45:</a> TC001 Move application import `poetry.console.commands.command.Command` into a type-checking block
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/console/application.py#L137'>src/poetry/console/application.py:137:9:</a> F841 Local variable `io` is assigned to but never used
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TC001 | 1 | 0 | 1 | 0 | 0 |
| F841 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @MichaReiser on 2024-12-06 12:39_

Any idea why there's an ecosystem change?

---

_@MichaReiser reviewed on 2024-12-06 12:40_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_pathlib_with_suffix.rs`:114 on 2024-12-06 12:40_

I don't think we need the check here actually. We know it's less than u32 max because there's at least a `")` following after ;)

---

_Comment by @AlexWaygood on 2024-12-06 12:40_

> Any idea why there's an ecosystem change?

No clue. Neither of the differences being reported is relevant to the changes being made here.

---

_@AlexWaygood reviewed on 2024-12-06 12:43_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_pathlib_with_suffix.rs`:114 on 2024-12-06 12:43_

Gah, I forgot you could use `+` between two `TextSize` instances 🤦

---

_@AlexWaygood reviewed on 2024-12-06 12:48_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_pathlib_with_suffix.rs`:114 on 2024-12-06 12:48_

#14816

---
