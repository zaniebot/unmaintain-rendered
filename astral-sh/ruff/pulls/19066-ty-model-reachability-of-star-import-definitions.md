```yaml
number: 19066
title: "[ty] Model reachability of star import definitions for nonlocal lookups"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-728
created_at: 2025-07-01T08:29:23Z
updated_at: 2025-07-01T09:06:39Z
url: https://github.com/astral-sh/ruff/pull/19066
synced_at: 2026-01-12T15:56:31Z
```

# [ty] Model reachability of star import definitions for nonlocal lookups

---

_@sharkdp_

## Summary

Temporarily modify `UseDefMapBuilder::reachability` for star imports in order for new definitions to pick up the right reachability. This was already working for `UseDefMapBuilder::place_states`, but not for `UseDefMapBuilder::reachable_definitions`.

closes https://github.com/astral-sh/ty/issues/728

## Test Plan

Regression test

---

_Label `ty` added by @sharkdp on 2025-07-01 08:29_

---

_Comment by @github-actions[bot] on 2025-07-01 08:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
alerta (https://github.com/alerta/alerta)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~117MB

rich (https://github.com/Textualize/rich)
-     memo fields = ~106MB
+     memo fields = ~117MB

trio (https://github.com/python-trio/trio)
- error[invalid-type-form] src/trio/_core/_run.py:138:41: List literals are not allowed in this context in a type expression: Did you mean `list[BaseExcT]`?
- error[invalid-type-form] src/trio/_core/_run.py:1344:28: List literals are not allowed in this context in a type expression
+ warning[unused-ignore-comment] src/trio/_core/_run.py:1510:59: Unused blanket `type: ignore` directive
- error[invalid-type-form] src/trio/_core/_run.py:1512:27: List literals are not allowed in this context in a type expression
+ warning[unused-ignore-comment] src/trio/_core/_run.py:1739:54: Unused blanket `type: ignore` directive
- error[invalid-type-form] src/trio/_core/_run.py:1737:40: List literals are not allowed in this context in a type expression
- error[invalid-type-form] src/trio/_core/_run.py:1737:50: List literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
- error[invalid-type-form] src/trio/_core/_run.py:1738:44: List literals are not allowed in this context in a type expression
- error[invalid-type-form] src/trio/_core/_run.py:1738:54: List literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
- error[invalid-type-form] src/trio/_core/_run.py:1933:28: List literals are not allowed in this context in a type expression
- error[invalid-type-form] src/trio/_core/_run.py:2065:28: List literals are not allowed in this context in a type expression
- error[invalid-type-form] src/trio/_core/_run.py:2132:28: List literals are not allowed in this context in a type expression
- error[invalid-argument-type] src/trio/_core/_run.py:2685:48: Argument is incorrect: Expected `bool`, found `@Todo(Support for `typing.TypeAlias`) | Literal[0] | list[Unknown]`
+ error[invalid-argument-type] src/trio/_core/_run.py:2685:48: Argument is incorrect: Expected `bool`, found `@Todo(Support for `typing.TypeAlias`) | list[Unknown]`
- Found 890 diagnostics
+ Found 882 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~207MB
+     memo fields = ~189MB

pylox (https://github.com/sco1/pylox)
-     memo fields = ~54MB
+     memo fields = ~49MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~171MB
+     memo fields = ~156MB

freqtrade (https://github.com/freqtrade/freqtrade)
-     memo fields = ~304MB
+     memo fields = ~276MB

static-frame (https://github.com/static-frame/static-frame)
-     memo fields = ~334MB
+     memo fields = ~304MB

openlibrary (https://github.com/internetarchive/openlibrary)
-     memo fields = ~171MB
+     memo fields = ~189MB

rotki (https://github.com/rotki/rotki)
-     memo fields = ~490MB
+     memo fields = ~445MB

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- TOTAL MEMORY USAGE: ~717MB
+ TOTAL MEMORY USAGE: ~652MB

```
</details>


---

_Marked ready for review by @sharkdp on 2025-07-01 08:41_

---

_Review requested from @carljm by @sharkdp on 2025-07-01 08:41_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-01 08:41_

---

_Review requested from @dcreager by @sharkdp on 2025-07-01 08:41_

---

_Comment by @sharkdp on 2025-07-01 09:06_

Merging this without review because https://github.com/astral-sh/ruff/pull/18955 depends on it, and I'd like to see the updated ecosystem results.

---

_Merged by @sharkdp on 2025-07-01 09:06_

---

_Closed by @sharkdp on 2025-07-01 09:06_

---

_Branch deleted on 2025-07-01 09:06_

---
