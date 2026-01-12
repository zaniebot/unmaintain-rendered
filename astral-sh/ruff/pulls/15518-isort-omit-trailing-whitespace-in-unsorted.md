```yaml
number: 15518
title: "[`isort`] Omit trailing whitespace in `unsorted-imports` (`I001`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - diagnostics
assignees: []
merged: true
base: main
head: isort-diagnostic-range
created_at: 2025-01-15T22:28:36Z
updated_at: 2025-01-18T17:08:58Z
url: https://github.com/astral-sh/ruff/pull/15518
synced_at: 2026-01-12T15:55:51Z
```

# [`isort`] Omit trailing whitespace in `unsorted-imports` (`I001`)

---

_@dylwil3_

## Summary
The fix range for sorting imports accounts for trailing whitespace, but we should only show the trimmed range to the user when displaying the diagnostic. So this PR changes the diagnostic range.

Closes #15504 

## Test Plan

Reviewed snapshot changes

---

_Label `diagnostics` added by @dylwil3 on 2025-01-15 22:28_

---

_Comment by @dylwil3 on 2025-01-15 22:30_

Questions:

1. Is it kosher to have the fix range larger than the diagnostic range? Is there some logic that assumes containment goes the other way?
2. Is this a breaking change because of suppression comments?
3. Would you like me to include end of line comments?

---

_Review requested from @BurntSushi by @dylwil3 on 2025-01-15 22:31_

---

_Comment by @github-actions[bot] on 2025-01-15 22:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+5 -5 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+5 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/gapminder/data.py#L5'>examples/server/app/gapminder/data.py:5:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/gapminder/data.py#L5'>examples/server/app/gapminder/data.py:5:5:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/model.py#L46'>src/bokeh/model/model.py:46:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/model.py#L46'>src/bokeh/model/model.py:46:5:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/util.py#L167'>src/bokeh/model/util.py:167:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/util.py#L167'>src/bokeh/model/util.py:167:5:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/connection.py#L28'>src/bokeh/server/connection.py:28:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/connection.py#L28'>src/bokeh/server/connection.py:28:5:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/test_cross.py#L34'>tests/test_cross.py:34:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/test_cross.py#L34'>tests/test_cross.py:34:5:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| I001 | 10 | 5 | 5 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -5 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+5 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/gapminder/data.py#L5'>examples/server/app/gapminder/data.py:5:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/gapminder/data.py#L5'>examples/server/app/gapminder/data.py:5:5:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/model.py#L46'>src/bokeh/model/model.py:46:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/model.py#L46'>src/bokeh/model/model.py:46:5:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/util.py#L167'>src/bokeh/model/util.py:167:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/util.py#L167'>src/bokeh/model/util.py:167:5:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/connection.py#L28'>src/bokeh/server/connection.py:28:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/connection.py#L28'>src/bokeh/server/connection.py:28:5:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/test_cross.py#L34'>tests/test_cross.py:34:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/test_cross.py#L34'>tests/test_cross.py:34:5:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| I001 | 10 | 5 | 5 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @dylwil3 on 2025-01-15 22:34_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/isort/snapshots/ruff_linter__rules__isort__tests__force_single_line_force_single_line.py.snap`:32 on 2025-01-16 06:19_

Not strictly incorrect but I think I liked the old one better 

---

_@MichaReiser reviewed on 2025-01-16 06:21_

Thanks. 

I kind of liked that the old range included the entire line instead of just the expression range. It would be nice if we can preserve that behavior 

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/isort/snapshots/ruff_linter__rules__isort__tests__force_single_line_force_single_line.py.snap`:32 on 2025-01-16 15:56_

I think I like the new one since it's a little more specific. But I do feel like the old one is more aesthetically pleasing.

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/isort/snapshots/ruff_linter__rules__isort__tests__lines_after_imports.pyi.snap`:15 on 2025-01-16 15:56_

Nice!

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/isort/snapshots/ruff_linter__rules__isort__tests__skip.py.snap`:51 on 2025-01-16 15:57_

Another nice improvement I think.

---

_@BurntSushi reviewed on 2025-01-16 15:57_

---

_@dylwil3 reviewed on 2025-01-18 17:08_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/isort/snapshots/ruff_linter__rules__isort__tests__force_single_line_force_single_line.py.snap`:32 on 2025-01-18 17:08_

I think I agree that the new behavior feels more "correct" about where the actual violation is, so I think I'll break the tie in favor of the smaller range. Happy to do a follow up if the community doesn't like it!

---

_Merged by @dylwil3 on 2025-01-18 17:08_

---

_Closed by @dylwil3 on 2025-01-18 17:08_

---
