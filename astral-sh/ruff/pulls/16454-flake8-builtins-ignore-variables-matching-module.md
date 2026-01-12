```yaml
number: 16454
title: "[flake8-builtins] Ignore variables matching module attribute names (A001)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: A001
created_at: 2025-03-01T22:12:24Z
updated_at: 2025-03-03T10:10:24Z
url: https://github.com/astral-sh/ruff/pull/16454
synced_at: 2026-01-12T15:55:55Z
```

# [flake8-builtins] Ignore variables matching module attribute names (A001)

---

_@VascoSch92_

This PR (partially) addresses issue #16373 


---

_Comment by @github-actions[bot] on 2025-03-01 22:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L1002'>src/bokeh/command/subcommands/serve.py:1002:1:</a> A001 Variable `__doc__` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/serialization.py#L89'>src/bokeh/util/serialization.py:89:1:</a> A001 Variable `__doc__` is shadowing a Python builtin
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| A001 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L1002'>src/bokeh/command/subcommands/serve.py:1002:1:</a> A001 Variable `__doc__` is shadowing a Python builtin
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/serialization.py#L89'>src/bokeh/util/serialization.py:89:1:</a> A001 Variable `__doc__` is shadowing a Python builtin
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| A001 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Renamed from "[flake8-builtins] Exclude Variables Matching Module Attribute Names from Violation Reports (A001)" to "[flake8-builtins] Exclude Variables Matching Module Attribute Names (A001)" by @MichaReiser on 2025-03-03 10:09_

---

_Renamed from "[flake8-builtins] Exclude Variables Matching Module Attribute Names (A001)" to "[flake8-builtins] Ignore variables matching module attribute names (A001)" by @MichaReiser on 2025-03-03 10:10_

---

_@MichaReiser approved on 2025-03-03 10:10_

Thank you

---

_Label `bug` added by @MichaReiser on 2025-03-03 10:10_

---

_Label `rule` added by @MichaReiser on 2025-03-03 10:10_

---

_Merged by @MichaReiser on 2025-03-03 10:10_

---

_Closed by @MichaReiser on 2025-03-03 10:10_

---
