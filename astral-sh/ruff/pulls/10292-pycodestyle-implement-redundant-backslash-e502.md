```yaml
number: 10292
title: "[`pycodestyle`] Implement `redundant-backslash` (`E502`)"
type: pull_request
state: merged
author: augustelalande
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: redundant-backslash
created_at: 2024-03-08T04:03:50Z
updated_at: 2024-04-30T18:12:20Z
url: https://github.com/astral-sh/ruff/pull/10292
synced_at: 2026-01-12T15:55:31Z
```

# [`pycodestyle`] Implement `redundant-backslash` (`E502`)

---

_@augustelalande_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implements the [redundant-backslash](https://pycodestyle.pycqa.org/en/latest/intro.html#error-codes) rule (E502) from pycodestyle.

## Test Plan

New fixture has been added

Part of #2402


---

_Review requested from @MichaReiser by @augustelalande on 2024-03-08 04:03_

---

_Renamed from "[`pycodestyle`] Implement redundant-backslash` (`E502`)" to "[`pycodestyle`] Implement `redundant-backslash` (`E502`)" by @augustelalande on 2024-03-08 04:28_

---

_Comment by @github-actions[bot] on 2024-03-08 04:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/webdriver.py#L198'>src/bokeh/io/webdriver.py:198:105:</a> E502 [*] Redundant backslash
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/webdriver.py#L199'>src/bokeh/io/webdriver.py:199:105:</a> E502 [*] Redundant backslash
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/contexts.py#L304'>src/bokeh/server/contexts.py:304:102:</a> E502 [*] Redundant backslash
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/session.py#L90'>src/bokeh/server/session.py:90:96:</a> E502 [*] Redundant backslash
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E502 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/redundant_backslash.rs`:92 on 2024-03-09 00:00_

We have a struct called `Indexer` that already stores the locations of all continuations (i.e., the line positions following a `\`). I'm wondering if we could instead iterate over the token stream, identify parenthesized ranges, and then search for `continuation_lines` offsets within parenthesized ranges?

---

_@charliermarsh reviewed on 2024-03-09 00:00_

---

_@augustelalande reviewed on 2024-03-10 00:13_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/redundant_backslash.rs`:92 on 2024-03-10 00:13_

Done

---

_@charliermarsh approved on 2024-03-12 01:14_

Thanks, this looks good to me.

---

_Label `preview` added by @charliermarsh on 2024-03-12 01:14_

---

_Label `rule` added by @charliermarsh on 2024-03-12 01:14_

---

_Merged by @charliermarsh on 2024-03-12 01:15_

---

_Closed by @charliermarsh on 2024-03-12 01:15_

---

_Branch deleted on 2024-03-12 02:45_

---

_Comment by @Avasam on 2024-04-30 18:12_

One more step towards https://github.com/astral-sh/ruff/discussions/9057 🎉

---
