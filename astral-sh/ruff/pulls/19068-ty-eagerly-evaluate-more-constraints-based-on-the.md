```yaml
number: 19068
title: "[ty] Eagerly evaluate more constraints based on the raw AST"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/more-eager-constraints
created_at: 2025-07-01T09:20:45Z
updated_at: 2025-07-01T10:17:47Z
url: https://github.com/astral-sh/ruff/pull/19068
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Eagerly evaluate more constraints based on the raw AST

---

_Pull request opened by @AlexWaygood on 2025-07-01 09:20_

In particular, `while 1:` is a [surprisingly common idiom](https://grep.app/search?f.lang=Python&case=true&q=while+1%3A) (due to the fact that the `bool` type was a surprisingly late addition to Python); and also, the truthiness of float literals and complex literals is actually easier to determine from the raw AST than the evaluated type

---

_Label `ty` added by @AlexWaygood on 2025-07-01 09:20_

---

_Comment by @github-actions[bot] on 2025-07-01 09:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB

alerta (https://github.com/alerta/alerta)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~117MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~207MB
+     memo fields = ~189MB

mkosi (https://github.com/systemd/mkosi)
- TOTAL MEMORY USAGE: ~129MB
+ TOTAL MEMORY USAGE: ~117MB
-     memo fields = ~106MB
+     memo fields = ~97MB

tornado (https://github.com/tornadoweb/tornado)
-     memo fields = ~142MB
+     memo fields = ~129MB

vision (https://github.com/pytorch/vision)
-     memo fields = ~276MB
+     memo fields = ~304MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~156MB
+     memo fields = ~171MB

static-frame (https://github.com/static-frame/static-frame)
-     memo fields = ~334MB
+     memo fields = ~304MB

```
</details>


---

_Renamed from "[ty] Eagerly evaluate more constraints" to "[ty] Eagerly evaluate more constraints based on the raw AST" by @AlexWaygood on 2025-07-01 09:28_

---

_Comment by @AlexWaygood on 2025-07-01 09:34_

A bit of a wash on the benchmarks? A bunch of the instrumented ones are showing small slowdowns, but some of the walltime ones are showing small speedups. No changes bigger than 1%.

Semantically, though, I do think it's confusing if `while True:` is understood differently to `while 1:`, even if that's only a temporary state we're in

---

_Marked ready for review by @AlexWaygood on 2025-07-01 09:34_

---

_Review requested from @carljm by @AlexWaygood on 2025-07-01 09:34_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-01 09:34_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-01 09:34_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/builder.rs`:571 on 2025-07-01 10:09_

Should we limit the complexity and the potential inconsistency between this and type inference here? For example, whether or not empty tuples should be considered always-falsy was a recent matter of debate. And it seems extremely unlikely that someone would use something like `if f""` of `if 0j`.

I would personally say the `ast::Number::Int(_)` change and `ast::Expr::NoneLiteral` seem like reasonable additions. Everything else, I would remove again. It only adds code complexity, and we do perform additional work for no gain (if those patterns are never used).

---

_@sharkdp reviewed on 2025-07-01 10:09_

---

_@sharkdp approved on 2025-07-01 10:09_

Thanks!

---

_@AlexWaygood reviewed on 2025-07-01 10:11_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/builder.rs`:571 on 2025-07-01 10:11_

make sense, thanks!

---

_Merged by @AlexWaygood on 2025-07-01 10:17_

---

_Closed by @AlexWaygood on 2025-07-01 10:17_

---

_Branch deleted on 2025-07-01 10:17_

---
