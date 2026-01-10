```yaml
number: 19019
title: "[ty] Remove `ScopedExpressionId`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: micha/ast-ids
created_at: 2025-06-28T19:37:43Z
updated_at: 2025-07-02T15:57:34Z
url: https://github.com/astral-sh/ruff/pull/19019
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Remove `ScopedExpressionId`

---

_Pull request opened by @MichaReiser on 2025-06-28 19:37_

## Summary

The motivation of `ScopedExpressionId` was that we have an expression identifier that's local to a scope and, therefore, unlikely to change if a user makes changes in another scope. A local identifier like this has the advantage that query results may remain unchanged even if other parts of the file change, which in turn allows Salsa to short-circuit dependent queries. 

However, I noticed that we aren't using `ScopedExpressionId` in a place where it's important that the identifier is local. It's main use is inside `infer` which we always run for the entire file. The one exception to this is `Unpack` but unpack runs as part of `infer`. 

Edit: The above isn't entirely correct. We used ScopedExpressionId in TypeInference which is a query result. Now using ExpressionNodeKey does mean that a change to the AST invalidates most if not all TypeInference results of a single file. Salsa then has to run all dependent queries to see if they're affected by this change even if the change was local to another scope. 

If this locality proves to be important I suggest that we create two queries on top of TypeInference: one that returns the expression map which is mainly used in the linter and type inference and a second that returns all remaining fields. This should give us a similar optimization at a much lower cost 

I also considered remove `ScopedUseId` but I believe that one is still useful because using `ExpressionNodeKey` for it instead would mean that all `UseDefMap` change when a single AST node changes. Whether this is important is something difficult to assess. I'm simply not familiar enough with the `UseDefMap`. If the locality doesn't matter for the `UseDefMap`, then a similar change could be made and `bindings_by_use` could be changed to an `FxHashMap<UseId, Bindings>` where `UseId` is a thin wrapper around `NodeKey`. 

Closes https://github.com/astral-sh/ty/issues/721



---

_Label `internal` added by @MichaReiser on 2025-06-28 19:37_

---

_Label `ty` added by @MichaReiser on 2025-06-28 19:37_

---

_Comment by @github-actions[bot] on 2025-06-28 19:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
-     memo fields = ~17MB
+     memo fields = ~15MB

janus (https://github.com/aio-libs/janus)
- TOTAL MEMORY USAGE: ~28MB
+ TOTAL MEMORY USAGE: ~25MB

com2ann (https://github.com/ilevkivskyi/com2ann)
- TOTAL MEMORY USAGE: ~41MB
+ TOTAL MEMORY USAGE: ~37MB

pyp (https://github.com/hauntsaninja/pyp)
-     memo fields = ~41MB
+     memo fields = ~37MB

zipp (https://github.com/jaraco/zipp)
- TOTAL MEMORY USAGE: ~30MB
+ TOTAL MEMORY USAGE: ~28MB

pegen (https://github.com/we-like-parsers/pegen)
- TOTAL MEMORY USAGE: ~45MB
+ TOTAL MEMORY USAGE: ~41MB

beartype (https://github.com/beartype/beartype)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~88MB

attrs (https://github.com/python-attrs/attrs)
- TOTAL MEMORY USAGE: ~80MB
+ TOTAL MEMORY USAGE: ~72MB

aiortc (https://github.com/aiortc/aiortc)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~88MB

dedupe (https://github.com/dedupeio/dedupe)
-     memo fields = ~72MB
+     memo fields = ~66MB

graphql-core (https://github.com/graphql-python/graphql-core)
-     memo fields = ~129MB
+     memo fields = ~117MB

svcs (https://github.com/hynek/svcs)
- TOTAL MEMORY USAGE: ~80MB
+ TOTAL MEMORY USAGE: ~72MB

black (https://github.com/psf/black)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~129MB

alerta (https://github.com/alerta/alerta)
-     memo fields = ~97MB
+     memo fields = ~88MB

downforeveryone (https://github.com/rpdelaney/downforeveryone)
-     memo fields = ~34MB
+     memo fields = ~30MB

rich (https://github.com/Textualize/rich)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~129MB

PyGithub (https://github.com/PyGithub/PyGithub)
-     memo metadata = ~17MB
+     memo metadata = ~15MB

pydantic (https://github.com/pydantic/pydantic)
- TOTAL MEMORY USAGE: ~156MB
+ TOTAL MEMORY USAGE: ~142MB

mkosi (https://github.com/systemd/mkosi)
- TOTAL MEMORY USAGE: ~129MB
+ TOTAL MEMORY USAGE: ~117MB

imagehash (https://github.com/JohannesBuchner/imagehash)
- TOTAL MEMORY USAGE: ~49MB
+ TOTAL MEMORY USAGE: ~45MB

flake8 (https://github.com/pycqa/flake8)
-     memo fields = ~66MB
+     memo fields = ~60MB

pylox (https://github.com/sco1/pylox)
-     memo fields = ~54MB
+     memo fields = ~49MB

apprise (https://github.com/caronc/apprise)
-     memo fields = ~251MB
+     memo fields = ~228MB

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~106MB

alectryon (https://github.com/cpitclaudel/alectryon)
-     memo fields = ~13MB
+     memo fields = ~11MB

poetry (https://github.com/python-poetry/poetry)
-     memo metadata = ~23MB
+     memo metadata = ~21MB

bandersnatch (https://github.com/pypa/bandersnatch)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~80MB

python-sop (https://gitlab.com/dkg/python-sop)
-     memo fields = ~25MB
+     memo fields = ~23MB

boostedblob (https://github.com/hauntsaninja/boostedblob)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~97MB

freqtrade (https://github.com/freqtrade/freqtrade)
-     memo fields = ~304MB
+     memo fields = ~276MB

AutoSplit (https://github.com/Toufool/AutoSplit)
- TOTAL MEMORY USAGE: ~228MB
+ TOTAL MEMORY USAGE: ~207MB

pywin32 (https://github.com/mhammond/pywin32)
-     memo fields = ~368MB
+     memo fields = ~334MB

altair (https://github.com/vega/altair)
-     memo fields = ~251MB
+     memo fields = ~228MB

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- TOTAL MEMORY USAGE: ~304MB
+ TOTAL MEMORY USAGE: ~276MB

pycryptodome (https://github.com/Legrandin/pycryptodome)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~129MB
-     memo metadata = ~17MB
+     memo metadata = ~15MB

openlibrary (https://github.com/internetarchive/openlibrary)
-     memo fields = ~189MB
+     memo fields = ~171MB

manticore (https://github.com/trailofbits/manticore)
-     memo fields = ~539MB
+     memo fields = ~490MB

meson (https://github.com/mesonbuild/meson)
-     memo metadata = ~54MB
+     memo metadata = ~49MB
-     memo fields = ~334MB
+     memo fields = ~304MB

zulip (https://github.com/zulip/zulip)
-     memo fields = ~652MB
+     memo fields = ~593MB

scipy (https://github.com/scipy/scipy)
- TOTAL MEMORY USAGE: ~1271MB
+ TOTAL MEMORY USAGE: ~1156MB

sympy (https://github.com/sympy/sympy)
-     memo fields = ~1538MB
+     memo fields = ~1399MB

```
</details>


---

_Comment by @charliermarsh on 2025-06-28 19:42_

Woah those report diffs are amazing.

---

_Comment by @MichaReiser on 2025-06-28 19:44_

> Woah those report diffs are amazing.

That's @ibraheemdev's work. He deserves a shoutout in tomorrow's meeting but Notion is having a bad day :(

---

_Comment by @codspeed-hq[bot] on 2025-06-28 19:47_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fast-ids?runnerMode=Instrumentation)

### Merging #19019 will **improve performances by 4.59%**

<sub>Comparing <code>micha/ast-ids</code> (d078951) with <code>main</code> (9218bf7)</sub>



### Summary

`⚡ 4` improvements  
`✅ 35` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` ty_check_file[cold] `` | 124.1 ms | 119 ms | +4.29% |
| ⚡ | `` ty_micro[complex_constrained_attributes_2] `` | 651.1 ms | 624.9 ms | +4.2% |
| ⚡ | `` anyio `` | 899.3 ms | 864.6 ms | +4% |
| ⚡ | `` attrs `` | 375.7 ms | 359.2 ms | +4.59% |


---

_Comment by @codspeed-hq[bot] on 2025-06-28 19:49_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fast-ids?runnerMode=WallTime)

### Merging #19019 will **improve performances by 7.79%**

<sub>Comparing <code>micha/ast-ids</code> (d078951) with <code>main</code> (9218bf7)</sub>



### Summary

`⚡ 7` improvements  
`✅ 1` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` large[sympy] `` | 53.5 s | 50.9 s | +5.2% |
| ⚡ | `` medium[colour-science] `` | 8.7 s | 8.3 s | +5.21% |
| ⚡ | `` medium[pandas] `` | 34.1 s | 32.5 s | +4.89% |
| ⚡ | `` multithreaded[pydantic] `` | 8.6 s | 8 s | +7.79% |
| ⚡ | `` small[altair] `` | 6.1 s | 5.8 s | +5.23% |
| ⚡ | `` small[freqtrade] `` | 9.2 s | 8.8 s | +4.43% |
| ⚡ | `` small[tanjun] `` | 4 s | 3.9 s | +4.12% |


---

_Renamed from "[ty] Delete `AstIds`" to "[ty] Remove `HasScopedExpressionId`" by @MichaReiser on 2025-06-28 19:57_

---

_Renamed from "[ty] Remove `HasScopedExpressionId`" to "[ty] Remove `ScopedExpressionId`" by @MichaReiser on 2025-06-28 19:57_

---

_Marked ready for review by @MichaReiser on 2025-06-28 20:09_

---

_Review requested from @carljm by @MichaReiser on 2025-06-28 20:09_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-28 20:09_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-28 20:09_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-28 20:09_

---

_Label `internal` removed by @MichaReiser on 2025-06-28 20:09_

---

_Label `performance` added by @MichaReiser on 2025-06-28 20:10_

---

_Comment by @MichaReiser on 2025-06-28 20:33_

This PR makes me wonder if we should try to split our semantic index incrementally (scope by scope). This won't help much for first party modules where we need all scopes but it can reduce the amount of data that we collect for third party modules AND it could maybe even replaces `exported_names` because other files would then only depend on the index of the global scope

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-29 17:13_

---

_@sharkdp approved on 2025-07-02 15:57_

This is great — less code and a clear performance win!

---

_Merged by @sharkdp on 2025-07-02 15:57_

---

_Closed by @sharkdp on 2025-07-02 15:57_

---

_Branch deleted on 2025-07-02 15:57_

---
