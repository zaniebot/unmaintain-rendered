```yaml
number: 20316
title: "[ty] Add tests for protocols with generic method members"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/generic-method-tests
created_at: 2025-09-09T13:56:59Z
updated_at: 2025-09-09T16:51:38Z
url: https://github.com/astral-sh/ruff/pull/20316
synced_at: 2026-01-10T17:46:22Z
```

# [ty] Add tests for protocols with generic method members

---

_Pull request opened by @AlexWaygood on 2025-09-09 13:56_

## Summary

Protocols can have method members, and those method members can have generic contexts. The generic context of a protocol's method members can be scoped to the class or the method itself.

We currently have some "accidental integration tests" for generic method members in our named-tuple test suite, but no explicit unit tests in `protocols.md`. This PR adds them.

## Test Plan

`cargo test -p ty_python_semantic --test=mdtest`


---

_Label `testing` added by @AlexWaygood on 2025-09-09 13:57_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-09 13:57_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-09 13:57_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-09 13:57_

---

_Label `ty` added by @AlexWaygood on 2025-09-09 13:57_

---

_Renamed from "[ty] Add explicit unit tests for protocols with method members with function-scoped generic contexts" to "[ty] Add explicit unit tests for protocols with generic method members" by @AlexWaygood on 2025-09-09 13:57_

---

_Renamed from "[ty] Add explicit unit tests for protocols with generic method members" to "[ty] Add tests for protocols with generic method members" by @AlexWaygood on 2025-09-09 13:57_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1850 on 2025-09-09 15:23_

"non-fully-static => no subtyping relation" is not a correct conclusion anymore since https://github.com/astral-sh/ruff/pull/18799 has been merged, I think?

The test assertions are still fine, I think, but we could consider changing the comment?

---

_@sharkdp approved on 2025-09-09 15:25_

Looks good, thank you.

---

_@AlexWaygood reviewed on 2025-09-09 15:39_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1850 on 2025-09-09 15:39_

uff, yes, thanks. I can never remember what our new rule is for when something can be assignable but not a subtype 🫣 let me read up on that again...

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1850 on 2025-09-09 15:43_

```suggestion
# `NewStyle` is implicitly `NewStyle[Unknown]`,
# and there exist fully static materializations of `NewStyle[Unknown]`
# where `Nominal` would not be a subtype of the given materialization,
# hence there is no subtyping relation:
```

---

_@AlexWaygood reviewed on 2025-09-09 15:43_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-09-09 16:39_

---

_Merged by @AlexWaygood on 2025-09-09 16:44_

---

_Closed by @AlexWaygood on 2025-09-09 16:44_

---

_Branch deleted on 2025-09-09 16:44_

---

_Comment by @github-actions[bot] on 2025-09-09 16:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+265 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/36fc820f27a25df2997cc2c85010d5caa02e8211/providers/common/sql/src/airflow/providers/common/sql/hooks/sql.pyi#L112'>providers/common/sql/src/airflow/providers/common/sql/hooks/sql.pyi:112:10:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/apache/airflow/blob/36fc820f27a25df2997cc2c85010d5caa02e8211/providers/common/sql/src/airflow/providers/common/sql/hooks/sql.pyi#L146'>providers/common/sql/src/airflow/providers/common/sql/hooks/sql.pyi:146:10:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/apache/airflow/blob/36fc820f27a25df2997cc2c85010d5caa02e8211/providers/common/sql/src/airflow/providers/common/sql/hooks/sql.pyi#L156'>providers/common/sql/src/airflow/providers/common/sql/hooks/sql.pyi:156:10:</a> UP043 [*] Unnecessary default type arguments
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+262 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/argparse.pyi#L315'>stdlib/argparse.pyi:315:60:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/codecs.pyi#L198'>stdlib/codecs.pyi:198:83:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/codecs.pyi#L199'>stdlib/codecs.pyi:199:85:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/concurrent/futures/process.pyi#L100'>stdlib/concurrent/futures/process.pyi:100:53:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/email/message.pyi#L133'>stdlib/email/message.pyi:133:23:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/lib2to3/fixes/fix_except.pyi#L9'>stdlib/lib2to3/fixes/fix_except.pyi:9:42:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/lib2to3/fixes/fix_import.pyi#L8'>stdlib/lib2to3/fixes/fix_import.pyi:8:32:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/lib2to3/fixes/fix_imports.pyi#L11'>stdlib/lib2to3/fixes/fix_imports.pyi:11:35:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/lib2to3/fixes/fix_metaclass.pyi#L11'>stdlib/lib2to3/fixes/fix_metaclass.pyi:11:29:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/lib2to3/fixes/fix_renames.pyi#L10'>stdlib/lib2to3/fixes/fix_renames.pyi:10:24:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/lib2to3/fixes/fix_urllib.pyi#L8'>stdlib/lib2to3/fixes/fix_urllib.pyi:8:24:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/lib2to3/refactor.pyi#L72'>stdlib/lib2to3/refactor.pyi:72:10:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/lib2to3/refactor.pyi#L73'>stdlib/lib2to3/refactor.pyi:73:63:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/pathlib/__init__.pyi#L183'>stdlib/pathlib/__init__.pyi:183:80:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/pathlib/__init__.pyi#L184'>stdlib/pathlib/__init__.pyi:184:81:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/pathlib/__init__.pyi#L186'>stdlib/pathlib/__init__.pyi:186:41:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/pathlib/__init__.pyi#L187'>stdlib/pathlib/__init__.pyi:187:42:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/pathlib/__init__.pyi#L202'>stdlib/pathlib/__init__.pyi:202:26:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/sqlite3/__init__.pyi#L367'>stdlib/sqlite3/__init__.pyi:367:61:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/sqlite3/__init__.pyi#L369'>stdlib/sqlite3/__init__.pyi:369:31:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/tokenize.pyi#L154'>stdlib/tokenize.pyi:154:60:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/tokenize.pyi#L155'>stdlib/tokenize.pyi:155:53:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/traceback.pyi#L114'>stdlib/traceback.pyi:114:90:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/traceback.pyi#L235'>stdlib/traceback.pyi:235:96:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/traceback.pyi#L237'>stdlib/traceback.pyi:237:52:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/traceback.pyi#L240'>stdlib/traceback.pyi:240:90:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/traceback.pyi#L242'>stdlib/traceback.pyi:242:44:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/xml/etree/ElementPath.pyi#L11'>stdlib/xml/etree/ElementPath.pyi:11:72:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/xml/etree/ElementPath.pyi#L14'>stdlib/xml/etree/ElementPath.pyi:14:80:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/xml/etree/ElementPath.pyi#L35'>stdlib/xml/etree/ElementPath.pyi:35:90:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/xml/etree/ElementTree.pyi#L109'>stdlib/xml/etree/ElementTree.pyi:109:47:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/xml/etree/ElementTree.pyi#L113'>stdlib/xml/etree/ElementTree.pyi:113:80:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/xml/etree/ElementTree.pyi#L114'>stdlib/xml/etree/ElementTree.pyi:114:27:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/xml/etree/ElementTree.pyi#L161'>stdlib/xml/etree/ElementTree.pyi:161:47:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stdlib/xml/etree/ElementTree.pyi#L171'>stdlib/xml/etree/ElementTree.pyi:171:80:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stubs/JACK-Client/jack/__init__.pyi#L240'>stubs/JACK-Client/jack/__init__.pyi:240:39:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stubs/PyScreeze/pyscreeze/__init__.pyi#L111'>stubs/PyScreeze/pyscreeze/__init__.pyi:111:6:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stubs/PyScreeze/pyscreeze/__init__.pyi#L123'>stubs/PyScreeze/pyscreeze/__init__.pyi:123:6:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stubs/PyScreeze/pyscreeze/__init__.pyi#L205'>stubs/PyScreeze/pyscreeze/__init__.pyi:205:6:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stubs/PyScreeze/pyscreeze/__init__.pyi#L217'>stubs/PyScreeze/pyscreeze/__init__.pyi:217:6:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stubs/Pygments/pygments/filters/__init__.pyi#L10'>stubs/Pygments/pygments/filters/__init__.pyi:10:26:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stubs/Pygments/pygments/formatters/__init__.pyi#L22'>stubs/Pygments/pygments/formatters/__init__.pyi:22:29:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stubs/Pygments/pygments/plugin.pyi#L24'>stubs/Pygments/pygments/plugin.pyi:24:29:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stubs/Pygments/pygments/plugin.pyi#L25'>stubs/Pygments/pygments/plugin.pyi:25:33:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stubs/Pygments/pygments/plugin.pyi#L26'>stubs/Pygments/pygments/plugin.pyi:26:29:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stubs/Pygments/pygments/plugin.pyi#L27'>stubs/Pygments/pygments/plugin.pyi:27:30:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stubs/antlr4-python3-runtime/antlr4/ParserRuleContext.pyi#L32'>stubs/antlr4-python3-runtime/antlr4/ParserRuleContext.pyi:32:46:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stubs/antlr4-python3-runtime/antlr4/RuleContext.pyi#L26'>stubs/antlr4-python3-runtime/antlr4/RuleContext.pyi:26:30:</a> UP043 [*] Unnecessary default type arguments
+ <a href='https://github.com/python/typeshed/blob/de819980041bf9f86ba120e1f070456ce8800d5a/stubs/assertpy/assertpy/assertpy.pyi#L64'>stubs/assertpy/assertpy/assertpy.pyi:64:26:</a> UP043 [*] Unnecessary default type arguments
... 213 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP043 | 265 | 265 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
