```yaml
number: 13890
title: "Fix E221 and E222 to flag missing or extra whitespace around `==` operator"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - rule
  - preview
assignees: []
merged: true
base: main
head: micha/fix-pycodestyle-eqeq
created_at: 2024-10-23T12:45:51Z
updated_at: 2024-10-23T13:02:31Z
url: https://github.com/astral-sh/ruff/pull/13890
synced_at: 2026-01-10T20:59:37Z
```

# Fix E221 and E222 to flag missing or extra whitespace around `==` operator

---

_Pull request opened by @MichaReiser on 2024-10-23 12:45_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/13886

## Test Plan

Added tests


---

_Label `bug` added by @MichaReiser on 2024-10-23 12:46_

---

_Label `rule` added by @MichaReiser on 2024-10-23 12:46_

---

_Label `preview` added by @MichaReiser on 2024-10-23 12:46_

---

_@dhruvmanila approved on 2024-10-23 12:50_

---

_Comment by @github-actions[bot] on 2024-10-23 12:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+29 -0 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+29 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/models/test_sources.py#L114'>tests/integration/models/test_sources.py:114:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/models/test_sources.py#L123'>tests/integration/models/test_sources.py:123:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/models/test_sources.py#L70'>tests/integration/models/test_sources.py:70:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/models/test_sources.py#L79'>tests/integration/models/test_sources.py:79:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L104'>tests/integration/widgets/tables/test_source_updates.py:104:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L149'>tests/integration/widgets/tables/test_source_updates.py:149:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L157'>tests/integration/widgets/tables/test_source_updates.py:157:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L203'>tests/integration/widgets/tables/test_source_updates.py:203:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L211'>tests/integration/widgets/tables/test_source_updates.py:211:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L250'>tests/integration/widgets/tables/test_source_updates.py:250:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L259'>tests/integration/widgets/tables/test_source_updates.py:259:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L298'>tests/integration/widgets/tables/test_source_updates.py:298:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L309'>tests/integration/widgets/tables/test_source_updates.py:309:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L320'>tests/integration/widgets/tables/test_source_updates.py:320:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L346'>tests/integration/widgets/tables/test_source_updates.py:346:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L402'>tests/integration/widgets/tables/test_source_updates.py:402:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L423'>tests/integration/widgets/tables/test_source_updates.py:423:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L457'>tests/integration/widgets/tables/test_source_updates.py:457:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L478'>tests/integration/widgets/tables/test_source_updates.py:478:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L512'>tests/integration/widgets/tables/test_source_updates.py:512:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L533'>tests/integration/widgets/tables/test_source_updates.py:533:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L561'>tests/integration/widgets/tables/test_source_updates.py:561:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L583'>tests/integration/widgets/tables/test_source_updates.py:583:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L611'>tests/integration/widgets/tables/test_source_updates.py:611:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L623'>tests/integration/widgets/tables/test_source_updates.py:623:26:</a> E222 [*] Multiple spaces after operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/tables/test_source_updates.py#L96'>tests/integration/widgets/tables/test_source_updates.py:96:26:</a> E222 [*] Multiple spaces after operator
... 2 additional changes omitted for rule E222
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test___init___io.py#L53'>tests/unit/bokeh/io/test___init___io.py:53:41:</a> E221 [*] Multiple spaces before operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test___init___io.py#L54'>tests/unit/bokeh/io/test___init___io.py:54:41:</a> E221 [*] Multiple spaces before operator
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E222 | 27 | 27 | 0 | 0 | 0 |
| E221 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @MichaReiser on 2024-10-23 13:02_

The ecosystem changes look correct.

---

_Merged by @MichaReiser on 2024-10-23 13:02_

---

_Closed by @MichaReiser on 2024-10-23 13:02_

---

_Branch deleted on 2024-10-23 13:02_

---
