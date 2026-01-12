```yaml
number: 18333
title: "[ty] Correctly handle calls to functions marked as returning `Never` / `NoReturn`"
type: pull_request
state: merged
author: abhijeetbodas2001
labels:
  - ty
assignees: []
merged: true
base: main
head: never
created_at: 2025-05-27T14:50:59Z
updated_at: 2025-07-05T06:26:15Z
url: https://github.com/astral-sh/ruff/pull/18333
synced_at: 2026-01-12T15:56:17Z
```

# [ty] Correctly handle calls to functions marked as returning `Never` / `NoReturn`

---

_@abhijeetbodas2001_

## Summary

`ty` does not understand that calls to functions which have been annotated as having a return type of `Never` / `NoReturn` are terminal.

This PR fixes that, by adding new reachability constraints when call expressions are seen. If the call expression evaluates to `Never`, the code following it will be considered to be unreachable. Note that, for adding these constraints, we only consider call expressions at the statement level, and that too only inside function scopes. This is because otherwise, the number of such constraints becomes too high, and evaluating them later on during type inference results in a major performance degradation.

Fixes https://github.com/astral-sh/ty/issues/180

## Test Plan

New mdtests.

## Ecosystem changes

This PR removes the following false-positives:
- "Function can implicitly return `None`, which is not assignable to ...".
- "Name `foo` used when possibly not defind" - because the branch in which it is not defined has a `NoReturn` call, or when `foo` was imported in a `try`, and the except had a `NoReturn` call.


---

_Comment by @abhijeetbodas2001 on 2025-05-27 14:51_

This isn't ready for review yet, but I wanted a `mypy_primer` run, and a place to write notes.

---

_Label `ty` added by @AlexWaygood on 2025-05-27 14:52_

---

_Comment by @AlexWaygood on 2025-05-27 16:44_

the crash in mypy_primer is unrelated; I've filed https://github.com/astral-sh/ruff/pull/18336 to fix it

---

_Closed by @AlexWaygood on 2025-05-27 16:51_

---

_Reopened by @AlexWaygood on 2025-05-27 16:51_

---

_Comment by @abhijeetbodas2001 on 2025-05-27 17:00_

Does seem related now. Investigating.

---

_Comment by @AlexWaygood on 2025-05-27 17:03_

> Does seem related now. Investigating.

Hmm, I'm not so sure -- I feel like assertions from inside Salsa (a third-party library we use) should probably never trigger, no matter what we do in ty 😄 there could very well be a bug in this PR _as well_, but I think there's probably a bug in Salsa here too

---

_Comment by @AlexWaygood on 2025-05-27 17:06_

yeah, you're running into https://github.com/salsa-rs/salsa/issues/831

---

_Comment by @MichaReiser on 2025-05-27 17:07_

> yeah, you're running into [salsa-rs/salsa#831](https://github.com/salsa-rs/salsa/issues/831)

Which means that this PR introduces deeply nested cycles. We may want to take a closer look if they can be avoided.

---

_Comment by @carljm on 2025-05-27 17:09_

The assertion error in `cycle.rs` is a known issue with cycle iteration in Salsa that @MichaReiser has already been looking at. I suspect the only relation to this diff is that it can cause more cycles, particularly with deferred name resolution.

---

_Comment by @abhijeetbodas2001 on 2025-05-27 17:53_

Thanks for the explanation.
This is also timing out when analyzing https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/resources/test/fixtures/flake8_use_pathlib/PTH210.py via `run_corpus_tests`, but I cannot reproduce the long run-time if I copy the contents from that file and run it as an `mdtest`. Interesting.

---

_Comment by @AlexWaygood on 2025-05-27 18:00_

The corpus tests run in a pretty different way to our mdtests. Our mdtests try to emulate how ty would run if you executed it from the command line. But the corpus tests attempt to probe the type of each sub-expression in a file's AST, which is somewhat different to what ty does when it's run from the command line. (Many subexpressions can actually end up being irrelevant when ty is being run from the command line, so it skips analyzing them altogether.)

---

_Comment by @carljm on 2025-05-27 18:02_

[This](https://github.com/astral-sh/ruff/blob/main/crates/ty_project/tests/check.rs#L172) is the extra step that runs for corpus tests. We currently don't have an easy way to run that on an arbitrary code file from CLI, but it would be a good thing to add.

---

_Closed by @AlexWaygood on 2025-06-01 15:41_

---

_Reopened by @AlexWaygood on 2025-06-01 15:41_

---

_Comment by @abhijeetbodas2001 on 2025-06-01 15:50_

Is closing+re-opening the PR good enough, or would a rebase on `main` be required?

---

_Comment by @AlexWaygood on 2025-06-01 15:53_

> Is closing+re-opening the PR enough good enough, or would a rebase on `main` be required?

Closing+reopening should be sufficient. GitHub CI on PRs doesn't run directly on the PR branch -- it runs on the result of a merge commit of the PR branch with the target branch.

---

_Comment by @codspeed-hq[bot] on 2025-06-02 06:47_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/abhijeetbodas2001%3Anever?runnerMode=Instrumentation)

### Merging #18333 will **degrade performances by 7.96%**

<sub>Comparing <code>abhijeetbodas2001:never</code> (c9c7890) with <code>main</code> (411cccb)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 38` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` hydra-zen `` | 779.6 ms | 847 ms | -7.96% |


---

_@abhijeetbodas2001 reviewed on 2025-06-02 07:08_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2228 on 2025-06-02 07:08_

I'm not adding the new constraints in stub files now.
Current intention is just to please CI, until the visibility constraints refactor lands.

However, I do think that, this work of evaluating the `ReturnsNever` constraints is unnecessary in stub files?

---

_Renamed from "INCOMPLETE: Correctly handle calls to functions returning `Never` / `NoReturn`" to "[ty] INCOMPLETE: Correctly handle calls to functions returning `Never` / `NoReturn`" by @MichaReiser on 2025-06-12 06:55_

---

_Comment by @sharkdp on 2025-06-17 08:07_

@abhijeetbodas2001 I saw that you just rebased this on top of main after the reachability constraints PR was merged. Let me know if you have any questions or need help with this.

---

_@sharkdp reviewed on 2025-06-17 08:17_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2231 on 2025-06-17 08:17_

Why did you decide to save a reference to `func` here instead of saving a reference to the whole call expression? I think that would allow you to use the inferred type for the whole call expression (which could then be compared to `Never`). This would save you from going through all overloads and checking the annotations.

---

_@carljm reviewed on 2025-06-17 16:01_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2231 on 2025-06-17 16:01_

I think this was my suggestion. The idea here was that inferring a type for an entire call expression may be orders of magnitude more expensive, because it requires inferring types for every argument, checking assignability of all arguments, possibly running the full overload resolution algorithm (which can involve union expansion and exponentially growing numbers of argument-list evaluations), etc.

The most common case by far (and thus the one we most need to optimize) is the case where we call a function that cannot return `Never`. The second most common case is that we call a function which does not have overloads and always returns `Never`. In both of those cases we can short-circuit all the argument-related work by just inferring a type for `func` and checking its return type directly.

This optimization does add the complication that we have more work to do when `func` has overloads -- we need to walk all the overloads and see if any of them can return `Never`, and if so, then we actually need to resolve the full call and see if it does return `Never` at this particular call site.

---

_Comment by @carljm on 2025-06-17 16:08_

I think this PR was sort of waiting for https://github.com/astral-sh/ty/issues/210, to see if it significantly reduces the number of these new constraints that we need to evaluate in practice.

But given that the latest codspeed results here are showing <5% cold-check regression, we might be able to consider merging this without even waiting for that.

---

_@abhijeetbodas2001 reviewed on 2025-06-17 18:56_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2231 on 2025-06-17 18:56_

> inferring a type for an entire call expression may be orders of magnitude more expensive

That makes sense, but one thing I did not understand is, we do infer types of all expressions once when inferring the file as a whole right? If we turn all call expressions into `standalone_expression`s, can we not re-use the inference result from the first time? (I see that `infer_standalone_expression` makes use of salsa caching via `infer_expression_types`.)

Is the concern that that would lead to the many new salsa structs consuming a lot of memory?

---

_Comment by @abhijeetbodas2001 on 2025-06-17 18:56_

> @abhijeetbodas2001 I saw that you just rebased this on top of main after the reachability constraints PR was merged. Let me know if you have any questions or need help with this.

Thanks! I haven't gotten to investigating the outstanding failures here. I'll do that and get back.

---

_@carljm reviewed on 2025-06-17 19:56_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2231 on 2025-06-17 19:56_

> we do infer types of all expressions once when inferring the file as a whole right

But we don't do this for all files -- the file may be a third-party dependency, or a first-party file for which type checking was not requested on the CLI, in which case we do minimal inference of just the definitions we need.

You are right that if we are inferring types of all expressions anyway in the file, then the inference will be reused anyway.

---

_@sharkdp reviewed on 2025-06-23 06:53_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2231 on 2025-06-23 06:53_

If this turns out to be a required / valuable performance optimization, then maybe we could store references to both, the `func` *and* the full call expression? We could then use `func` for the fast-path, and fall back to inferring types for the full call expression whenever any complication arises (e.g. overloaded functions)?

---

_Comment by @codspeed-hq[bot] on 2025-06-24 06:08_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/abhijeetbodas2001%3Anever?runnerMode=WallTime)

### Merging #18333 will **degrade performances by 5.49%**

<sub>Comparing <code>abhijeetbodas2001:never</code> (c9c7890) with <code>main</code> (411cccb)</sub>



### Summary

`❌ 3` regressions  
`✅ 4` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/abhijeetbodas2001%3Anever?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` medium[colour-science] `` | 8.5 s | 9 s | -5.49% |
| ❌ | `` medium[pandas] `` | 33.7 s | 35.1 s | -4.02% |
| ❌ | `` small[freqtrade] `` | 4.5 s | 4.8 s | -4.94% |


---

_Comment by @abhijeetbodas2001 on 2025-06-24 06:17_

The mypy-primer failure is on `nox`, which uses `packaging`. My changes seem to have triggered inference for the file https://github.com/pypa/packaging/blob/main/src/packaging/requirements.py. If checked directly with `ty check requirements.py`, this fails on the latest alpha as well, with ``infer_definition_types(Id(3455)): execute: too many cycle iterations``.

@AlexWaygood I saw your comment at https://github.com/astral-sh/ty/issues/423#issuecomment-2971714486, and I think this might also be related to https://github.com/astral-sh/ty/issues/256?

---

A minimal reproduction, which fails on this branch, but not on the latest alpha is this:

```py
import packaging.requirements


def _(req: packaging.requirements.Requirement):
    req.name

```

My understanding is that this block will try to infer the type of `requirements.py/Requirement.name`, and as part of that, end up inferring the `_parser.py` file which Alex mentioned in the above comment. This inference would be triggered because of the additional constraints on this branch.

---

_@abhijeetbodas2001 reviewed on 2025-06-24 06:34_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/resources/mdtest/terminal_statements.md`:620 on 2025-06-24 06:34_

This does not work because of https://github.com/astral-sh/ty/issues/685 which I filed recently. I think that would need more discussion and better fixed in a follow up PR. I will _unmark_ this PR from fixing https://github.com/astral-sh/ty/issues/180, lest that main issue get closed automatically.

---

_@abhijeetbodas2001 reviewed on 2025-06-26 17:55_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md`:171 on 2025-06-26 17:55_

This code was affected by https://github.com/astral-sh/ty/issues/685, so I did this hack to side-step it and make the test pass.

---

_Comment by @github-actions[bot] on 2025-06-26 18:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
-     memo fields = ~41MB
+     memo fields = ~45MB

parso (https://github.com/davidhalter/parso)
-     memo fields = ~49MB
+     memo fields = ~54MB

com2ann (https://github.com/ilevkivskyi/com2ann)
- TOTAL MEMORY USAGE: ~37MB
+ TOTAL MEMORY USAGE: ~41MB

python-chess (https://github.com/niklasf/python-chess)
-     memo metadata = ~7MB
+     memo metadata = ~8MB

pegen (https://github.com/we-like-parsers/pegen)
- error[invalid-return-type] src/pegen/__main__.py:27:6: Function can implicitly return `None`, which is not assignable to return type `tuple[Grammar, Parser, Tokenizer, ParserGenerator]`
- warning[possibly-unresolved-reference] src/pegen/first_sets.py:144:37: Name `grammar` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_visualizer.py:59:31: Name `grammar` used when possibly not defined
- Found 50 diagnostics
+ Found 47 diagnostics
- TOTAL MEMORY USAGE: ~41MB
+ TOTAL MEMORY USAGE: ~45MB

nionutils (https://github.com/nion-software/nionutils)
-     memo metadata = ~5MB
+     memo metadata = ~6MB

attrs (https://github.com/python-attrs/attrs)
- TOTAL MEMORY USAGE: ~72MB
+ TOTAL MEMORY USAGE: ~80MB

beartype (https://github.com/beartype/beartype)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB

pyinstrument (https://github.com/joerick/pyinstrument)
- warning[possibly-unresolved-reference] pyinstrument/__main__.py:323:8: Name `renderer` used when possibly not defined
- warning[possibly-unresolved-reference] pyinstrument/__main__.py:392:19: Name `renderer` used when possibly not defined
- warning[possibly-unresolved-reference] pyinstrument/__main__.py:394:27: Name `renderer` used when possibly not defined
- warning[possibly-unresolved-reference] pyinstrument/__main__.py:397:17: Name `renderer` used when possibly not defined
- warning[possibly-unresolved-reference] pyinstrument/__main__.py:401:19: Name `renderer` used when possibly not defined
- error[invalid-return-type] pyinstrument/__main__.py:594:55: Function can implicitly return `None`, which is not assignable to return type `Session`
- Found 50 diagnostics
+ Found 44 diagnostics

Expression (https://github.com/cognitedata/Expression)
- error[invalid-return-type] expression/collections/maptree.py:253:53: Function can implicitly return `None`, which is not assignable to return type `tuple[Key, Value, Option[MapTreeLeaf[Key, Value]]]`
- error[invalid-return-type] expression/collections/maptree.py:493:27: Function can implicitly return `None`, which is not assignable to return type `tuple[Key, Value]`
- Found 234 diagnostics
+ Found 232 diagnostics
-     memo fields = ~49MB
+     memo fields = ~54MB

aioredis (https://github.com/aio-libs/aioredis)
-     memo metadata = ~3MB
+     memo metadata = ~4MB
-     memo fields = ~54MB
+     memo fields = ~60MB

koda-validate (https://github.com/keithasaurus/koda-validate)
- error[invalid-return-type] koda_validate/serialization/json_schema.py:302:6: Function can implicitly return `None`, which is not assignable to return type `dict[str, Unknown]`
- error[invalid-return-type] koda_validate/serialization/json_schema.py:409:6: Function can implicitly return `None`, which is not assignable to return type `dict[str, Unknown]`
- error[invalid-return-type] koda_validate/serialization/json_schema.py:492:6: Function can implicitly return `None`, which is not assignable to return type `dict[str, Unknown]`
- error[invalid-return-type] koda_validate/serialization/json_schema.py:503:6: Function can implicitly return `None`, which is not assignable to return type `dict[str, Unknown]`
- Found 44 diagnostics
+ Found 40 diagnostics

twine (https://github.com/pypa/twine)
-     memo metadata = ~2MB
+     memo metadata = ~3MB

werkzeug (https://github.com/pallets/werkzeug)
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:42:42: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:45:41: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:48:41: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:51:56: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:54:38: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:57:38: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:60:42: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:63:51: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:66:41: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:69:33: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:72:66: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:117:64: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:120:57: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:123:40: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:126:57: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:129:26: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:132:56: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:135:42: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:138:24: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:156:48: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:159:30: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:162:38: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:165:55: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:168:73: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:185:59: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:188:56: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:191:68: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:194:53: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:197:68: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:200:75: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:203:37: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:206:57: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:209:57: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:212:40: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:215:51: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:218:68: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:221:26: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:224:57: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:227:61: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/exceptions.py:875:69: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[unresolved-attribute] tests/test_exceptions.py:19:12: Type `BaseException` has no attribute `get_response`
- Found 426 diagnostics
+ Found 385 diagnostics
-     memo metadata = ~14MB
+     memo metadata = ~15MB
-     memo fields = ~129MB
+     memo fields = ~142MB

black (https://github.com/psf/black)
- warning[possibly-unresolved-reference] src/black/__init__.py:687:13: Name `sources` used when possibly not defined
- warning[possibly-unresolved-reference] src/black/__init__.py:694:16: Name `sources` used when possibly not defined
- warning[possibly-unresolved-reference] src/black/__init__.py:696:21: Name `sources` used when possibly not defined
- warning[possibly-unresolved-reference] src/black/__init__.py:710:25: Name `sources` used when possibly not defined
- Found 73 diagnostics
+ Found 69 diagnostics
- TOTAL MEMORY USAGE: ~129MB
+ TOTAL MEMORY USAGE: ~142MB

starlette (https://github.com/encode/starlette)
-     struct fields = ~4MB
+     struct fields = ~5MB

dulwich (https://github.com/dulwich/dulwich)
-     memo fields = ~117MB
+     memo fields = ~129MB

aiortc (https://github.com/aiortc/aiortc)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB

graphql-core (https://github.com/graphql-python/graphql-core)
-     memo fields = ~117MB
+     memo fields = ~129MB

python-htmlgen (https://github.com/srittau/python-htmlgen)
- TOTAL MEMORY USAGE: ~30MB
+ TOTAL MEMORY USAGE: ~34MB

svcs (https://github.com/hynek/svcs)
- TOTAL MEMORY USAGE: ~72MB
+ TOTAL MEMORY USAGE: ~80MB
-     struct metadata = ~1MB
+     struct metadata = ~2MB

comtypes (https://github.com/enthought/comtypes)
- warning[possibly-unresolved-reference] comtypes/clear_cache.py:47:20: Name `comtypes` used when possibly not defined
- Found 483 diagnostics
+ Found 482 diagnostics
-     memo metadata = ~10MB
+     memo metadata = ~11MB

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- TOTAL MEMORY USAGE: ~80MB
+ TOTAL MEMORY USAGE: ~88MB
-     memo metadata = ~5MB
+     memo metadata = ~6MB
-     memo fields = ~72MB
+     memo fields = ~80MB

alerta (https://github.com/alerta/alerta)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~117MB
-     struct metadata = ~3MB
+     struct metadata = ~4MB
-     struct fields = ~5MB
+     struct fields = ~6MB
-     memo metadata = ~9MB
+     memo metadata = ~13MB
-     memo fields = ~88MB
+     memo fields = ~97MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~207MB
+     memo fields = ~189MB

kopf (https://github.com/nolar/kopf)
-     struct fields = ~4MB
+     struct fields = ~5MB

sockeye (https://github.com/awslabs/sockeye)
- warning[possibly-unresolved-reference] sockeye/train.py:1141:37: Name `apex` used when possibly not defined
- Found 322 diagnostics
+ Found 321 diagnostics
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~106MB
-     memo metadata = ~9MB
+     memo metadata = ~10MB

strawberry (https://github.com/strawberry-graphql/strawberry)
- warning[possibly-unresolved-reference] strawberry/relay/utils.py:71:32: Name `type_name` used when possibly not defined
- Found 360 diagnostics
+ Found 359 diagnostics
-     struct fields = ~6MB
+     struct fields = ~7MB

trio (https://github.com/python-trio/trio)
- error[invalid-return-type] src/trio/_core/_tests/test_exceptiongroup_gc.py:19:18: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/trio/_core/_tests/test_exceptiongroup_gc.py:23:20: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/trio/_core/_tests/test_exceptiongroup_gc.py:31:18: Function always implicitly returns `None`, which is not assignable to return type `Never`
- warning[possibly-unresolved-reference] src/trio/_tests/test_exports.py:50:18: Name `run` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_exports.py:160:18: Name `PyLinter` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_exports.py:174:18: Name `jedi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_exports.py:391:22: Name `jedi` used when possibly not defined
- error[unresolved-attribute] src/trio/_tests/test_util.py:351:12: Type `BaseException` has no attribute `code`
- error[invalid-return-type] src/trio/_util.py:381:6: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/trio/testing/_fake_net.py:409:30: Function can implicitly return `None`, which is not assignable to return type `tuple[str, int] | tuple[str, int, int, int]`
- Found 811 diagnostics
+ Found 801 diagnostics
-     struct fields = ~8MB
+     struct fields = ~9MB
-     memo metadata = ~19MB
+     memo metadata = ~21MB

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- warning[possibly-unresolved-reference] bson/__init__.py:365:51: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] bson/__init__.py:367:40: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] bson/__init__.py:369:16: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] bson/__init__.py:574:71: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] bson/__init__.py:576:40: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] bson/__init__.py:578:30: Name `value` used when possibly not defined
- error[invalid-return-type] pymongo/asynchronous/encryption.py:114:65: Function can implicitly return `None`, which is not assignable to return type `socket | _sslConn`
- error[invalid-return-type] pymongo/asynchronous/pool.py:622:72: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] pymongo/common.py:832:6: Function can implicitly return `None`, which is not assignable to return type `(...) -> Unknown`
+ warning[unused-ignore-comment] pymongo/pool_shared.py:319:72: Unused blanket `type: ignore` directive
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:321:13: Name `ssl_sock` used when possibly not defined
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:324:5: Name `ssl_sock` used when possibly not defined
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:325:12: Name `ssl_sock` used when possibly not defined
+ warning[unused-ignore-comment] pymongo/pool_shared.py:374:86: Unused blanket `type: ignore` directive
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:376:13: Name `transport` used when possibly not defined
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:379:38: Name `transport` used when possibly not defined
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:379:49: Name `protocol` used when possibly not defined
+ warning[unused-ignore-comment] pymongo/pool_shared.py:493:72: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] pymongo/pool_shared.py:542:72: Unused blanket `type: ignore` directive
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:495:13: Name `ssl_sock` used when possibly not defined
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:498:5: Name `ssl_sock` used when possibly not defined
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:499:12: Name `ssl_sock` used when possibly not defined
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:544:13: Name `ssl_sock` used when possibly not defined
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:547:5: Name `ssl_sock` used when possibly not defined
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:548:32: Name `ssl_sock` used when possibly not defined
- error[invalid-return-type] pymongo/synchronous/encryption.py:113:59: Function can implicitly return `None`, which is not assignable to return type `socket | _sslConn`
- error[invalid-return-type] pymongo/synchronous/pool.py:620:66: Function always implicitly returns `None`, which is not assignable to return type `Never`
- Found 449 diagnostics
+ Found 430 diagnostics
-     memo metadata = ~21MB
+     memo metadata = ~23MB

pybind11 (https://github.com/pybind/pybind11)
-     memo metadata = ~8MB
+     memo metadata = ~9MB

porcupine (https://github.com/Akuli/porcupine)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~129MB
-     memo metadata = ~8MB
+     memo metadata = ~10MB
-     memo fields = ~97MB
+     memo fields = ~106MB

nox (https://github.com/wntrblm/nox)
- error[invalid-return-type] nox/_cli.py:141:6: Function always implicitly returns `None`, which is not assignable to return type `Never`
- Found 24 diagnostics
+ Found 23 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[invalid-return-type] pydantic/json_schema.py:1608:59: Function can implicitly return `None`, which is not assignable to return type `bool`
- Found 766 diagnostics
+ Found 765 diagnostics
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~156MB

stone (https://github.com/dropbox/stone)
- warning[possibly-unresolved-reference] stone/cli.py:159:31: Name `logging_level` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:243:12: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:250:42: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:254:30: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:262:42: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:267:33: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:273:30: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:288:50: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:292:26: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:299:22: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:301:17: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:302:21: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:343:9: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:344:9: Name `backend_module` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:360:16: Name `api` used when possibly not defined
- Found 128 diagnostics
+ Found 113 diagnostics
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB
-     struct fields = ~4MB
+     struct fields = ~5MB
-     memo metadata = ~8MB
+     memo metadata = ~11MB
-     memo fields = ~72MB
+     memo fields = ~80MB

ppb-vector (https://github.com/ppb/ppb-vector)
- TOTAL MEMORY USAGE: ~37MB
+ TOTAL MEMORY USAGE: ~41MB

mkosi (https://github.com/systemd/mkosi)
- error[invalid-return-type] mkosi/__init__.py:1569:53: Function can implicitly return `None`, which is not assignable to return type `Path`
- error[invalid-return-type] mkosi/__init__.py:2351:71: Function can implicitly return `None`, which is not assignable to return type `list[Unknown]`
- warning[possibly-unresolved-reference] mkosi/completion.py:249:11: Name `func` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:910:8: Name `timestamp` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:913:12: Name `timestamp` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:925:8: Name `level` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:928:12: Name `level` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:940:8: Name `mode` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:943:8: Name `mode` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:946:12: Name `mode` used when possibly not defined
- error[invalid-return-type] mkosi/config.py:1068:35: Function can implicitly return `None`, which is not assignable to return type `SE`
- warning[possibly-unresolved-reference] mkosi/config.py:1447:8: Name `size` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:1447:22: Name `size` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:1448:54: Name `size` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:1450:26: Name `size` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:1451:44: Name `size` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:1453:12: Name `size` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:1471:8: Name `cid` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:1472:16: Name `cid` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:1474:12: Name `cid` used when possibly not defined
- error[invalid-argument-type] mkosi/config.py:1564:22: Argument is incorrect: Expected `KeySourceType`, found `KeySourceType | <class 'type'>`
- error[invalid-argument-type] mkosi/config.py:1594:30: Argument is incorrect: Expected `CertificateSourceType`, found `CertificateSourceType | <class 'type'>`
- warning[possibly-unresolved-reference] mkosi/config.py:4787:30: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:4788:36: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:4796:68: Name `result` used when possibly not defined
- error[invalid-return-type] mkosi/log.py:20:57: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] mkosi/mounts.py:33:35: Function can implicitly return `None`, which is not assignable to return type `Path`
- error[invalid-return-type] mkosi/qemu.py:141:56: Function can implicitly return `None`, which is not assignable to return type `int`
- error[invalid-return-type] mkosi/qemu.py:189:41: Function can implicitly return `None`, which is not assignable to return type `Path`
- warning[possibly-unresolved-reference] mkosi/run.py:232:19: Name `proc` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/run.py:233:13: Name `proc` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/run.py:235:13: Name `proc` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/run.py:238:13: Name `proc` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/run.py:242:13: Name `proc` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/run.py:243:26: Name `proc` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/user.py:82:12: Name `count` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/user.py:85:20: Name `count` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/user.py:85:57: Name `line` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/user.py:88:16: Name `start` used when possibly not defined
- error[invalid-return-type] mkosi/util.py:267:38: Function can implicitly return `None`, which is not assignable to return type `str`
- Found 130 diagnostics
+ Found 90 diagnostics
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~142MB
-     struct metadata = ~3MB
+     struct metadata = ~4MB
-     memo metadata = ~9MB
+     memo metadata = ~10MB
-     memo fields = ~106MB
+     memo fields = ~129MB

pylox (https://github.com/sco1/pylox)
-     memo metadata = ~4MB
+     memo metadata = ~5MB

ignite (https://github.com/pytorch/ignite)
- TOTAL MEMORY USAGE: ~207MB
+ TOTAL MEMORY USAGE: ~228MB
-     struct metadata = ~7MB
+     struct metadata = ~8MB
-     struct fields = ~9MB
+     struct fields = ~10MB
-     memo metadata = ~25MB
+     memo metadata = ~28MB

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
-     struct metadata = ~2MB
+     struct metadata = ~3MB
-     struct fields = ~4MB
+     struct fields = ~5MB

flake8 (https://github.com/pycqa/flake8)
-     struct fields = ~3MB
+     struct fields = ~4MB

websockets (https://github.com/aaugustin/websockets)
- warning[possibly-unresolved-reference] src/websockets/cli.py:116:33: Name `websocket` used when possibly not defined
- warning[possibly-unresolved-reference] src/websockets/cli.py:119:32: Name `websocket` used when possibly not defined
- warning[possibly-unresolved-reference] src/websockets/cli.py:139:11: Name `websocket` used when possibly not defined
- warning[possibly-unresolved-reference] src/websockets/cli.py:140:12: Name `websocket` used when possibly not defined
- warning[possibly-unresolved-reference] src/websockets/cli.py:140:49: Name `websocket` used when possibly not defined
- warning[possibly-unresolved-reference] src/websockets/cli.py:141:26: Name `websocket` used when possibly not defined
- warning[possibly-unresolved-reference] src/websockets/cli.py:141:48: Name `websocket` used when possibly not defined
- Found 72 diagnostics
+ Found 65 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- error[invalid-return-type] src/schemathesis/core/loaders.py:61:86: Function can implicitly return `None`, which is not assignable to return type `Response`
- warning[possibly-unresolved-reference] src/schemathesis/graphql/loaders.py:198:22: Name `schema` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:107:14: Name `expected_main` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:107:39: Name `expected_sub` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:108:17: Name `expected_main` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:108:34: Name `received_main` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:108:52: Name `expected_sub` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:109:17: Name `expected_main` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:109:42: Name `expected_sub` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:109:58: Name `received_sub` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:110:17: Name `expected_main` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:110:34: Name `received_main` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:110:52: Name `expected_sub` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:110:68: Name `received_sub` used when possibly not defined
- Found 304 diagnostics
+ Found 290 diagnostics
- TOTAL MEMORY USAGE: ~156MB
+ TOTAL MEMORY USAGE: ~171MB
-     memo metadata = ~14MB
+     memo metadata = ~15MB
-     memo fields = ~129MB
+     memo fields = ~142MB

pwndbg (https://github.com/pwndbg/pwndbg)
- TOTAL MEMORY USAGE: ~207MB
+ TOTAL MEMORY USAGE: ~228MB
-     struct fields = ~8MB
+     struct fields = ~9MB
-     memo metadata = ~21MB
+     memo metadata = ~23MB
-     memo fields = ~171MB
+     memo fields = ~189MB

bandersnatch (https://github.com/pypa/bandersnatch)
- TOTAL MEMORY USAGE: ~80MB
+ TOTAL MEMORY USAGE: ~88MB
-     struct metadata = ~2MB
+     struct metadata = ~3MB
-     memo metadata = ~6MB
+     memo metadata = ~7MB
-     memo fields = ~66MB
+     memo fields = ~72MB

optuna (https://github.com/optuna/optuna)
-     struct metadata = ~10MB
+     struct metadata = ~11MB
-     struct fields = ~15MB
+     struct fields = ~17MB
-     memo metadata = ~28MB
+     memo metadata = ~30MB

poetry (https://github.com/python-poetry/poetry)
- TOTAL MEMORY USAGE: ~207MB
+ TOTAL MEMORY USAGE: ~228MB
-     struct metadata = ~7MB
+     struct metadata = ~8MB
-     struct fields = ~9MB
+     struct fields = ~10MB
-     memo metadata = ~21MB
+     memo metadata = ~25MB
-     memo fields = ~171MB
+     memo fields = ~189MB

cloud-init (https://github.com/canonical/cloud-init)
- warning[possibly-unresolved-reference] cloudinit/analyze/__init__.py:302:12: Name `infh` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/analyze/__init__.py:302:18: Name `outfh` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/devel/hotplug_hook.py:325:36: Name `datasource` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1386:8: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1399:18: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1399:40: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1404:8: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1412:10: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1417:10: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1424:17: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1425:45: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1434:22: Name `functor` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1434:30: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1439:21: Name `name` used when possibly not defined
- Found 677 diagnostics
+ Found 663 diagnostics
- TOTAL MEMORY USAGE: ~368MB
+ TOTAL MEMORY USAGE: ~405MB
-     struct metadata = ~15MB
+     struct metadata = ~17MB
-     struct fields = ~19MB
+     struct fields = ~21MB
-     memo metadata = ~45MB
+     memo metadata = ~49MB

mkdocs (https://github.com/mkdocs/mkdocs)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~129MB
-     memo metadata = ~9MB
+     memo metadata = ~11MB
-     memo fields = ~97MB
+     memo fields = ~106MB

cki-lib (https://gitlab.com/cki-project/cki-lib)
-     memo fields = ~72MB
+     memo fields = ~80MB

tornado (https://github.com/tornadoweb/tornado)
- error[invalid-return-type] tornado/process.py:85:6: Function can implicitly return `None`, which is not assignable to return type `int`
- Found 245 diagnostics
+ Found 244 diagnostics
-     memo metadata = ~15MB
+     memo metadata = ~17MB
-     memo fields = ~129MB
+     memo fields = ~142MB

vision (https://github.com/pytorch/vision)
- TOTAL MEMORY USAGE: ~334MB
+ TOTAL MEMORY USAGE: ~368MB
-     memo metadata = ~37MB
+     memo metadata = ~41MB
-     memo fields = ~276MB
+     memo fields = ~304MB

jinja (https://github.com/pallets/jinja)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~106MB

dragonchain (https://github.com/dragonchain/dragonchain)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~106MB
-     struct metadata = ~3MB
+     struct metadata = ~4MB
-     memo metadata = ~8MB
+     memo metadata = ~10MB
-     memo fields = ~80MB
+     memo fields = ~88MB

yarl (https://github.com/aio-libs/yarl)
-     struct metadata = ~1MB
+     struct metadata = ~2MB

colour (https://github.com/colour-science/colour)
-     struct metadata = ~13MB
+     struct metadata = ~14MB
-     memo metadata = ~37MB
+     memo metadata = ~41MB

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~117MB
-     memo metadata = ~4MB
+     memo metadata = ~5MB

urllib3 (https://github.com/urllib3/urllib3)
-     struct metadata = ~5MB
+     struct metadata = ~6MB
-     memo metadata = ~14MB
+     memo metadata = ~15MB

boostedblob (https://github.com/hauntsaninja/boostedblob)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~106MB
-     struct metadata = ~2MB
+     struct metadata = ~3MB

schema_salad (https://github.com/common-workflow-language/schema_salad)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~156MB
-     struct fields = ~7MB
+     struct fields = ~8MB
-     memo metadata = ~11MB
+     memo metadata = ~14MB
-     memo fields = ~117MB
+     memo fields = ~129MB

isort (https://github.com/pycqa/isort)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB
-     memo fields = ~72MB
+     memo fields = ~80MB

scrapy (https://github.com/scrapy/scrapy)
- warning[possibly-unresolved-reference] docs/utils/linkfix.py:38:17: Name `output_lines` used when possibly not defined
- Found 1150 diagnostics
+ Found 1149 diagnostics
-     memo metadata = ~28MB
+     memo metadata = ~30MB
-     memo fields = ~189MB
+     memo fields = ~207MB

apprise (https://github.com/caronc/apprise)
- TOTAL MEMORY USAGE: ~304MB
+ TOTAL MEMORY USAGE: ~334MB
-     struct fields = ~11MB
+     struct fields = ~13MB
-     memo metadata = ~41MB
+     memo metadata = ~45MB
-     memo fields = ~251MB
+     memo fields = ~276MB

freqtrade (https://github.com/freqtrade/freqtrade)
-     struct metadata = ~10MB
+     struct metadata = ~11MB
-     memo metadata = ~25MB
+     memo metadata = ~28MB

AutoSplit (https://github.com/Toufool/AutoSplit)
- error[invalid-return-type] src/AutoSplit.py:989:31: Function always implicitly returns `None`, which is not assignable to return type `Never`
- warning[possibly-unresolved-reference] src/AutoSplit.py:1106:14: Name `exit_code` used when possibly not defined
- error[invalid-return-type] src/error_messages.py:248:58: Function always implicitly returns `None`, which is not assignable to return type `Never`
- Found 42 diagnostics
+ Found 39 diagnostics
-     struct fields = ~11MB
+     struct fields = ~13MB

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- error[type-assertion-failure] tests/test_frame.py:4198:9: Argument does not have asserted type `Never`
- error[no-matching-overload] tests/test_frame.py:4198:22: No overload of bound method `select_dtypes` matches arguments
- Found 2771 diagnostics
+ Found 2769 diagnostics
- TOTAL MEMORY USAGE: ~276MB
+ TOTAL MEMORY USAGE: ~304MB
-     struct metadata = ~8MB
+     struct metadata = ~9MB
-     struct fields = ~13MB
+     struct fields = ~14MB
-     memo metadata = ~21MB
+     memo metadata = ~28MB
-     memo fields = ~228MB
+     memo fields = ~251MB

PyGithub (https://github.com/PyGithub/PyGithub)
- TOTAL MEMORY USAGE: ~156MB
+ TOTAL MEMORY USAGE: ~171MB
-     struct metadata = ~5MB
+     struct metadata = ~6MB
-     struct fields = ~7MB
+     struct fields = ~8MB
-     memo metadata = ~15MB
+     memo metadata = ~21MB
-     memo fields = ~129MB
+     memo fields = ~142MB

cwltool (https://github.com/common-workflow-language/cwltool)
- warning[possibly-unbound-attribute] cwltool/main.py:455:9: Attribute `update` on type `None | Unknown | MutableMapping[str, @Todo(Inference of subscript on special form) | None] | dict[Unknown, Unknown]` is possibly unbound
- Found 138 diagnostics
+ Found 137 diagnostics
- TOTAL MEMORY USAGE: ~251MB
+ TOTAL MEMORY USAGE: ~276MB
-     struct fields = ~11MB
+     struct fields = ~13MB
-     memo metadata = ~17MB
+     memo metadata = ~19MB
-     memo fields = ~228MB
+     memo fields = ~251MB

pywin32 (https://github.com/mhammond/pywin32)
- warning[possibly-unresolved-reference] com/win32comext/adsi/demos/scp.py:485:8: Name `log_level` used when possibly not defined
- error[invalid-return-type] com/win32comext/axscript/client/framework.py:1198:71: Function always implicitly returns `None`, which is not assignable to return type `Never`
- warning[possibly-unresolved-reference] win32/Demos/NetValidatePasswordPolicy.py:105:39: Name `val_type` used when possibly not defined
- Found 1976 diagnostics
+ Found 1973 diagnostics
- TOTAL MEMORY USAGE: ~445MB
+ TOTAL MEMORY USAGE: ~490MB
-     struct metadata = ~19MB
+     struct metadata = ~21MB
-     struct fields = ~28MB
+     struct fields = ~30MB
-     memo metadata = ~49MB
+     memo metadata = ~54MB
-     memo fields = ~334MB
+     memo fields = ~368MB

psycopg (https://github.com/psycopg/psycopg)
- TOTAL MEMORY USAGE: ~207MB
+ TOTAL MEMORY USAGE: ~228MB
-     struct fields = ~10MB
+     struct fields = ~11MB
-     memo metadata = ~23MB
+     memo metadata = ~25MB
-     memo fields = ~171MB
+     memo fields = ~189MB

paasta (https://github.com/yelp/paasta)
- warning[possibly-unresolved-reference] paasta_tools/cleanup_expired_autoscaling_overrides.py:66:8: Name `configmap` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/cleanup_expired_autoscaling_overrides.py:72:12: Name `configmap` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/cleanup_expired_autoscaling_overrides.py:83:44: Name `configmap` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/cleanup_expired_autoscaling_overrides.py:136:9: Name `configmap` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/cleanup_expired_autoscaling_overrides.py:142:18: Name `configmap` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/cli/cmds/local_run.py:919:28: Name `secret_environment` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/cli/cmds/local_run.py:942:48: Name `chosen_port` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/cli/cmds/local_run.py:951:45: Name `chosen_port` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/cli/cmds/local_run.py:992:21: Name `chosen_port` used when possibly not defined
- error[invalid-return-type] paasta_tools/cli/cmds/logs.py:485:45: Function can implicitly return `None`, which is not assignable to return type `LogReader`
- error[invalid-return-type] paasta_tools/cli/cmds/logs.py:1150:54: Function can implicitly return `None`, which is not assignable to return type `str`
- warning[possibly-unresolved-reference] paasta_tools/setup_tron_namespace.py:140:12: Name `services` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/setup_tron_namespace.py:174:27: Name `services` used when possibly not defined
- error[invalid-return-type] paasta_tools/utils.py:4056:66: Function can implicitly return `None`, which is not assignable to return type `str`
- warning[possibly-unresolved-reference] paasta_tools/utils.py:4089:19: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/utils.py:4089:38: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/utils.py:4092:16: Name `result` used when possibly not defined
- Found 905 diagnostics
+ Found 888 diagnostics
-     memo metadata = ~21MB
+     memo metadata = ~23MB

pycryptodome (https://github.com/Legrandin/pycryptodome)
- TOTAL MEMORY USAGE: ~129MB
+ TOTAL MEMORY USAGE: ~142MB
-     memo metadata = ~15MB
+     memo metadata = ~19MB

pytest (https://github.com/pytest-dev/pytest)
- warning[possibly-unresolved-reference] src/_pytest/compat.py:147:18: Name `parameters` used when possibly not defined
- warning[possibly-unresolved-reference] src/_pytest/compat.py:160:61: Name `parameters` used when possibly not defined
- warning[possibly-unresolved-reference] src/_pytest/doctest.py:587:33: Name `module` used when possibly not defined
- warning[possibly-unresolved-reference] src/_pytest/doctest.py:587:41: Name `module` used when possibly not defined
- warning[possibly-unresolved-reference] src/_pytest/fixtures.py:224:49: Name `scoped_item_path` used when possibly not defined
- error[invalid-return-type] src/_pytest/mark/expression.py:200:29: Function can implicitly return `None`, which is not assignable to return type `expr`
- error[invalid-return-type] src/_pytest/nodes.py:99:36: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/_pytest/pytester.py:306:37: Function can implicitly return `None`, which is not assignable to return type `RecordedHookCall`
- error[invalid-return-type] src/_pytest/python.py:1009:68: Function can implicitly return `None`, which is not assignable to return type `str`
- error[invalid-return-type] src/_pytest/python.py:1029:59: Function always implicitly returns `None`, which is not assignable to return type `Never`
- warning[possibly-unresolved-reference] src/_pytest/python.py:1454:16: Name `arg_directness` used when possibly not defined
- error[invalid-return-type] src/_pytest/reports.py:197:10: Function can implicitly return `None`, which is not assignable to return type `tuple[str, Mapping[str, bool]]`
- warning[possibly-unresolved-reference] src/_pytest/reports.py:589:16: Name `reprentry` used when possibly not defined
- warning[possibly-unresolved-reference] src/_pytest/scope.py:83:16: Name `scope` used when possibly not defined
- warning[possibly-unresolved-reference] src/_pytest/skipping.py:158:12: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/_pytest/unittest.py:265:57: Name `excinfo` used when possibly not defined
- warning[possibly-unresolved-reference] testing/test_capture.py:992:28: Name `out` used when possibly not defined
- warning[possibly-unresolved-reference] testing/test_parseopt.py:320:26: Name `bash_version` used when possibly not defined
+ warning[unused-ignore-comment] testing/test_pytester.py:38:57: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] testing/test_pytester.py:53:58: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] testing/test_pytester.py:60:54: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] testing/test_pytester.py:71:28: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] testing/test_pytester.py:73:58: Unused blanket `type: ignore` directive
- warning[possibly-unresolved-reference] testing/test_session.py:177:19: Name `reprec` used when possibly not defined
- Found 542 diagnostics
+ Found 528 diagnostics
-     struct metadata = ~9MB
+     struct metadata = ~10MB
-     struct fields = ~13MB
+     struct fields = ~14MB
-     memo metadata = ~30MB
+     memo metadata = ~34MB
-     memo fields = ~189MB
+     memo fields = ~207MB

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- warning[possibly-unresolved-reference] mitmproxy/proxy/layers/http/_http2.py:187:68: Name `error_code` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/proxy/layers/http/_http3.py:128:68: Name `error_code` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/tools/main.py:85:23: Name `args` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/tools/main.py:91:43: Name `args` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/tools/main.py:93:16: Name `args` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/tools/main.py:96:16: Name `args` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/tools/main.py:100:20: Name `args` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/tools/main.py:102:73: Name `args` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/tools/main.py:104:37: Name `args` used when possibly not defined
- Found 1813 diagnostics
+ Found 1804 diagnostics
- TOTAL MEMORY USAGE: ~276MB
+ TOTAL MEMORY USAGE: ~304MB
-     struct fields = ~14MB
+     struct fields = ~15MB
-     memo metadata = ~34MB
+     memo metadata = ~41MB
-     memo fields = ~228MB
+     memo fields = ~251MB

rclip (https://github.com/yurijmikhalevich/rclip)
- warning[possibly-unresolved-reference] rclip/model.py:193:66: Name `images` used when possibly not defined
- Found 13 diagnostics
+ Found 12 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- warning[possibly-unresolved-reference] aiohttp/web.py:564:24: Name `module` used when possibly not defined
- warning[possibly-unresolved-reference] aiohttp/web.py:582:11: Name `func` used when possibly not defined
- Found 178 diagnostics
+ Found 176 diagnostics
-     struct fields = ~6MB
+     struct fields = ~7MB
-     memo metadata = ~11MB
+     memo metadata = ~13MB

static-frame (https://github.com/static-frame/static-frame)
-     struct metadata = ~14MB
+     struct metadata = ~15MB
-     struct fields = ~19MB
+     struct fields = ~21MB
-     memo metadata = ~49MB
+     memo metadata = ~54MB
-     memo fields = ~304MB
+     memo fields = ~334MB

bokeh (https://github.com/bokeh/bokeh)
- warning[possibly-unresolved-reference] src/bokeh/command/bootstrap.py:118:8: Name `ret` used when possibly not defined
- warning[possibly-unresolved-reference] src/bokeh/command/bootstrap.py:120:10: Name `ret` used when possibly not defined
- warning[possibly-unresolved-reference] src/bokeh/command/bootstrap.py:120:41: Name `ret` used when possibly not defined
- warning[possibly-unresolved-reference] src/bokeh/command/bootstrap.py:120:55: Name `ret` used when possibly not defined
- warning[possibly-unresolved-reference] src/bokeh/command/bootstrap.py:121:18: Name `ret` used when possibly not defined
- error[invalid-return-type] src/bokeh/command/util.py:58:43: Function always implicitly returns `None`, which is not assignable to return type `Never`
- Found 863 diagnostics
+ Found 857 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- warning[possibly-unresolved-reference] sphinx/cmd/build.py:377:23: Name `key` used when possibly not defined
- warning[possibly-unresolved-reference] sphinx/cmd/build.py:387:39: Name `key` used when possibly not defined
- Found 609 diagnostics
+ Found 607 diagnostics
-     struct metadata = ~10MB
+     struct metadata = ~11MB
-     memo metadata = ~34MB
+     memo metadata = ~37MB
-     memo fields = ~228MB
+     memo fields = ~251MB

openlibrary (https://github.com/internetarchive/openlibrary)
-     struct fields = ~10MB
+     struct fields = ~11MB
-     memo metadata = ~23MB
+     memo metadata = ~25MB
-     memo fields = ~171MB
+     memo fields = ~189MB

meson (https://github.com/mesonbuild/meson)
- error[invalid-return-type] mesonbuild/linkers/detect.py:40:69: Function can implicitly return `None`, which is not assignable to return type `DynamicLinker`
- warning[possibly-unresolved-reference] mesonbuild/linkers/detect.py:257:12: Name `linker` used when possibly not defined
- error[invalid-return-type] mesonbuild/mdevenv.py:160:41: Function can implicitly return `None`, which is not assignable to return type `int`
- warning[possibly-unresolved-reference] mesonbuild/minstall.py:733:16: Name `rc` used when possibly not defined
- warning[possibly-unresolved-reference] mesonbuild/minstall.py:734:82: Name `rc` used when possibly not defined
- warning[possibly-unresolved-reference] mesonbuild/minstall.py:735:26: Name `rc` used when possibly not defined
- warning[possibly-unresolved-reference] mesonbuild/scripts/depfixer.py:181:16: Name `ptrsize` used when possibly not defined
- warning[possibly-unresolved-reference] mesonbuild/scripts/depfixer.py:181:25: Name `is_le` used when possibly not defined
- warning[possibly-unresolved-reference] test cases/common/208 link custom/custom_stlib.py:41:17: Name `static_linker` used when possibly not defined
- warning[possibly-unresolved-reference] test cases/common/209 link custom_i single from multiple/generate_conflicting_stlibs.py:36:17: Name `static_linker` used when possibly not defined
- warning[possibly-unresolved-reference] test cases/common/210 link custom_i multiple from multiple/generate_stlibs.py:38:17: Name `static_linker` used when possibly not defined
- Found 930 diagnostics
+ Found 919 diagnostics
- TOTAL MEMORY USAGE: ~405MB
+ TOTAL MEMORY USAGE: ~445MB
-     struct fields = ~19MB
+     struct fields = ~21MB
-     memo metadata = ~49MB
+     memo metadata = ~60MB
-     memo fields = ~304MB
+     memo fields = ~334MB

streamlit (https://github.com/streamlit/streamlit)
- warning[possibly-unresolved-reference] lib/streamlit/logger.py:54:22: Name `log_level` used when possibly not defined
- warning[possibly-unresolved-reference] lib/streamlit/logger.py:57:25: Name `log_level` used when possibly not defined
- error[invalid-return-type] lib/streamlit/runtime/credentials.py:325:28: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] scripts/audit_frontend_licenses.py:120:44: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] scripts/audit_frontend_licenses.py:159:15: Function always implicitly returns `None`, which is not assignable to return type `Never`
- Found 3306 diagnostics
+ Found 3301 diagnostics
- TOTAL MEMORY USAGE: ~304MB
+ TOTAL MEMORY USAGE: ~334MB
-     struct fields = ~14MB
+     struct fields = ~15MB
-     memo metadata = ~30MB
+     memo metadata = ~34MB

materialize (https://github.com/MaterializeInc/materialize)
- error[invalid-return-type] misc/python/materialize/cli/run.py:69:15: Function can implicitly return `None`, which is not assignable to return type `int`
- Found 3216 diagnostics
+ Found 3215 diagnostics
-     struct metadata = ~17MB
+     struct metadata = ~19MB
-     memo metadata = ~37MB
+     memo metadata = ~41MB

prefect (https://github.com/PrefectHQ/prefect)
- warning[possibly-unresolved-reference] src/prefect/cli/block.py:402:41: Name `block_document` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/block.py:451:46: Name `block_type` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/block.py:455:17: Name `block_type` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/block.py:460:12: Name `latest_schema` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/block.py:463:59: Name `latest_schema` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/block.py:465:43: Name `latest_schema` used when possibly not defined
- error[invalid-return-type] src/prefect/cli/cloud/__init__.py:269:35: Function can implicitly return `None`, which is not assignable to return type `str`
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:534:26: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:539:16: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:542:53: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:550:39: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:554:46: Name `current_workspace` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:557:54: Name `current_workspace` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:562:8: Name `prompt_switch_workspace` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:566:17: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:572:12: Name `current_workspace` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:573:34: Name `current_workspace` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:574:18: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:575:34: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:639:14: Name `current_workspace` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:643:33: Name `current_workspace` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:660:47: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:667:70: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:699:30: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:705:20: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:709:35: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:722:78: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:727:60: Name `workspace` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:729:46: Name `workspace` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/concurrency_limit.py:85:23: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/concurrency_limit.py:92:66: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/concurrency_limit.py:98:28: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/concurrency_limit.py:102:17: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/concurrency_limit.py:103:17: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/concurrency_limit.py:104:40: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/concurrency_limit.py:104:59: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/concurrency_limit.py:105:40: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/concurrency_limit.py:105:59: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/config.py:52:12: Name `setting` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/config.py:53:53: Name `setting` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/config.py:56:12: Name `setting` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/config.py:58:28: Name `setting` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/config.py:62:25: Name `setting` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/config.py:62:36: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/config.py:81:42: Name `new_profile` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deploy.py:226:23: Name `files` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:111:12: Name `deployment` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:281:27: Name `deployment` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:283:12: Name `deployment` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:284:40: Name `deployment` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:295:39: Name `deployment` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:830:21: Name `start_time_raw` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:842:53: Name `start_time_raw` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:844:12: Name `start_time_parsed` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:845:69: Name `start_time_raw` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:847:57: Name `start_time_parsed` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:882:48: Name `scheduled_start_time` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:895:28: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:896:37: Name `scheduled_start_time` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:898:26: Name `human_dt_diff` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:900:43: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:904:20: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:905:26: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:906:29: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:914:48: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:916:13: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:1048:20: Name `key` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:1048:38: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:1050:46: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:1052:60: Name `key` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/flow.py:170:49: Name `runner_deployment` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/flow.py:173:29: Name `runner_deployment` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/flow.py:176:14: Name `runner_deployment` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/flow_run.py:92:29: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/flow_run.py:98:38: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/flow_run.py:254:8: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/flow_run.py:257:18: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/flow_run.py:355:45: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/global_concurrency_limit.py:124:26: Name `gcl_limit` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/global_concurrency_limit.py:135:34: Name `gcl_limit` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/global_concurrency_limit.py:384:85: Name `gcl` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/global_concurrency_limit.py:392:72: Name `gcl_id` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/task.py:98:33: Name `module_tasks` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/task.py:99:39: Name `module_tasks` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/task.py:100:26: Name `module_tasks` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/task_run.py:86:29: Name `task_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/task_run.py:92:38: Name `task_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/task_run.py:247:29: Name `task_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_pool.py:496:17: Name `work_pool` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_pool.py:500:56: Name `work_pool` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_pool.py:515:25: Name `work_pool` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_pool.py:676:47: Name `responses` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_pool.py:868:31: Name `block_type` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_pool.py:869:33: Name `block_schema` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_pool.py:928:47: Name `credentials_block_document` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_pool.py:1013:47: Name...*[Comment body truncated]*

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md`:171 on 2025-06-27 18:58_

Do you have a fuller understanding of how this is affected by https://github.com/astral-sh/ty/issues/685, and why it is surfacing in this PR? I don't see any calls returning `Never` here, and I don't see any terminal statements either, so the relationships aren't clear to me, nor is it clear to me why this change sidesteps the problem.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/terminal_statements.md`:620 on 2025-06-27 19:01_

I think it's still fine to close https://github.com/astral-sh/ty/issues/180 with this PR, as the general issue is fixed. We will still have https://github.com/astral-sh/ty/issues/685 open to track the particular issue (which isn't specific to calls returning `Never`).

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:721 on 2025-06-27 19:09_

I think we should at the very least support `Type::Callable` also. Ideally we'd support callable instances, bound methods, etc as well. I think the way to do this would be adding a `Type::into_callable_type -> Option<Type>` that always returns `Some(Type::Callable(...))` or `None` -- most of the pieces needed for that are already in place. For clarity it could be added in a separate PR (either before or after this one).

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:720 on 2025-06-27 19:12_

No we shouldn't add a panic here, we should silently return something and let type checking emit a diagnostic for a bad call.

It's an interesting question what we should return. In principle it makes sense to return `AlwaysTrue` (calling a non-callable will raise an error and thus terminate control flow). But we don't do that for other diagnostics that are high-confidence will-raise-an-error, so it seems inconsistent to do it here. Thus I think we should probably return `AlwaysFalse`.

---

_@carljm reviewed on 2025-06-27 19:16_

This is impressively little code! Thanks for working on this, and for your patience.

I think really the only blocker to landing this as a solid initial step is the performance impact; it's pretty bad right now on real-sized projects :/

I think it is worth waiting (a bit longer) to see the impact of https://github.com/astral-sh/ruff/pull/18955 here, as that should remove another frequent case where we would see a lot of these extra reachability constraints unnecessarily evaluated.

Beyond that, we might need to do some profiling to understand where this is causing us to spend a lot of time, and how we might be able to mitigate it.

---

_Comment by @abhijeetbodas2001 on 2025-06-28 09:39_

Thank you for the review! I shall address your comments in the next few days, and mark this as not-draft once done (I also need to verify the ecosystem diagnostic diff before that). Please ignore any pings from this PR till then -- I might need to push multiple times.

And the code here is tiny because David already did most of the foundational work so as to make this change easy to implement :)


---

_Renamed from "[ty] INCOMPLETE: Correctly handle calls to functions returning `Never` / `NoReturn`" to "[ty] Correctly handle calls to functions marked as returning `Never` / `NoReturn`" by @abhijeetbodas2001 on 2025-06-28 09:40_

---

_@abhijeetbodas2001 reviewed on 2025-06-28 15:23_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md`:171 on 2025-06-28 15:23_

Edit: this issue was likely fixed by https://github.com/astral-sh/ruff/pull/18955, and this hack is no longer required. This also means the below reasoning is incorrect.

<details>
<summary>Incorrect reasoning</summary>

Without this change to the test here, the test gives a "possibly unbound" error on this PR.

Ignoring the new `ReturnsNever` constraints for a while, consider this example:

```py
def f1(flag: bool, cond: bool):
  class C1:
    if flag:

      if cond:  # extra block
        raise

      attr = DataDescriptor()


    def f(self):
      self.attr = "normal"

  reveal_type(C1().attr)
```

This also triggers a "possibly unbound" error on `main` today (which I believe is not what we want).

The above example (without the `ReturnsNever` constraints) can be thought of as similar to the example in this test suite, but with the new `ReturnsNever` constrains, which I'll copy below from `main`:

```py
def f1(flag: bool):
  class C1:
    if flag:
      attr = DataDescriptor()   # original version, without my hack


    def f(self):
      self.attr = "normal"

  reveal_type(C1().attr)
```

The similarity is that, in both cases, there is a terminal statement with a condition. On this branch, the terminal statement is a call returning `Never`, with the condition (which is not yet evaluated when building the semantic index) that the thing being called should be annotated as returning `NoReturn`, while on `main` right now, with the example with `cond` I shared above, the terminal statemtent is `raise` and the condition (again, unevaluated when building the semantic index) is `cond`.

This is hand-wavy, and I don't have a very solid understanding on why this manifests as the "possibly unbound" error, since I haven't dug into the implementation of the descriptor fall-back described in this test suite. But the above parallel strongly suggests that this issue is related to https://github.com/astral-sh/ty/issues/685.

The hack I did side-steps this because it makes it so that the constraint of "does not return `Never`" is applied _outside_ the `if`, and independent of the snapshot-merge issue.

</details>

---

_@abhijeetbodas2001 reviewed on 2025-06-30 22:43_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md`:171 on 2025-06-30 22:43_

Edit: this was also fixed by https://github.com/astral-sh/ruff/pull/18955

I was looking at the `mypy_primer` diff, and there are some new diagnostics for "Attribute `foo` of type `bar` is possibly unbound". The MRE for them is the following:
```py
  def _(flag: bool):
      class C:
          def __init__(self, x: int):
              self.x = x

          if flag:
              print()  # could be any function call

      c = C(1)
      c.x  # possibly unbound
 ```

This smells similar to the above issue, and I now feel it would be worth digging deeper to figure out what's going on here. I shall do that.

---

_Closed by @abhijeetbodas2001 on 2025-07-01 15:07_

---

_Reopened by @abhijeetbodas2001 on 2025-07-01 15:07_

---

_Comment by @sharkdp on 2025-07-01 15:30_

> I think it is worth waiting (a bit longer) to see the impact of #18955 here, as that should remove another frequent case where we would see a lot of these extra reachability constraints unnecessarily evaluated.

That PR is merged now, so it might be valuable to rebase this branch onto main

---

_Comment by @abhijeetbodas2001 on 2025-07-01 17:24_

Thank you! There were some new false positive diagnostics which had come up in an earlier run of this PR. It seems your PR also fixed those. Example at https://github.com/astral-sh/ruff/pull/18333#discussion_r2176095181.

---

_Comment by @sharkdp on 2025-07-02 08:17_

I was thinking about a few approaches to improve the performance here. The main problem seems to be the sheer amount of reachability constraints that we record. In principle, *every* call expression could have a return type of `Never` and should therefore be analyzed. And we could go even further and also analyze every attribute access. *In principle*, the attribute could be a descriptor and it's `__get__` method could return `Never`. That seems ridiculous, but maybe illustrates the point that we may need to cut scope here?

One radical way to cut scope would be to only analyze call expressions that are at the statement level. This would mean that we could still handle a standalone `sys.exit(1)` call, but not something like `… if … else sys.exit(1)`, `_ = sys.exit(1)` or `return sys.exit(1)`. I tried that approach at https://github.com/astral-sh/ruff/pull/19085 and it's *much* faster. The only significant performance drop is for `hydra-zen` with -8%. Everything else is below 5%.

This is not very elegant, but maybe a reasonable first step? We could always expand the scope by recording the constraints in more situations (ternary expressions might be a reasonable addition, but I didn't find any examples where `sys.exit` specifically was used in a ternary).

---

_Comment by @carljm on 2025-07-02 17:39_

Thanks @sharkdp for the further exploration. I think it makes sense to start with the statement-level-only version, and consider expanding it carefully if/as we see issues crop up with more deeply nested calls returning `Never`.

---

_Comment by @sharkdp on 2025-07-02 17:46_

@abhijeetbodas2001 Feel free to cherry-pick 2eae76b6e190723843854158663377bb360d6172 onto this branch.

---

_Comment by @carljm on 2025-07-02 19:56_

I do still think it's _also_ worth exploring the idea that when resolving imports, we ignore some kinds of reachability constraints (call-Never constraints for sure, maybe also assertion constraints?) that are particularly likely to accumulate at end-of-scope for almost any symbol. I think that should dramatically reduce the number of these constraints we have to evaluate, and in particular, the number of them we have to evaluate in third-party code, that may trigger further type inference we could otherwise avoid entirely.

An alternative version of cutting scope here that would be even simpler to experiment with, but would I think have similar effects, would be to never add these call-Never constraints at all in non-function scopes. (Today, non-function scopes are the only ones where we use end-of-scope symbol resolution). I think this way of cutting scope might more closely match what other type checkers do and don't support? At least pyright doesn't ever seem to respect Never-returning calls as a constraint when inferring types of class attributes or imported names. (But within a function, it does respect Never-returning calls that aren't top-level statements.)

---

_@abhijeetbodas2001 reviewed on 2025-07-04 15:40_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:720 on 2025-07-04 15:40_

That makes sense. I changed this to return `AlwaysFalse`. It seems earlier I had the impression that this branch should be unreachable, hence the question about adding a panic. But I now get that that impression is clearly wrong, since semantic indexing will happily add a constraint here for `random_thing_which_is_not_a_function()`, by looking at just the AST :)

---

_@abhijeetbodas2001 reviewed on 2025-07-04 15:41_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:721 on 2025-07-04 15:41_

This now uses the new `into_callable` which was added in https://github.com/astral-sh/ruff/pull/19130.

---

_@abhijeetbodas2001 reviewed on 2025-07-04 15:42_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/resources/mdtest/terminal_statements.md`:620 on 2025-07-04 15:42_

Changed the PR description back to "fixes".
Once this is merged, I shall link to the new issue from the old issue as well.

---

_@abhijeetbodas2001 reviewed on 2025-07-04 15:43_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2231 on 2025-07-04 15:43_

I changed the `ReturnsNever` constraint to store references to both the callable and the call expression, and have implemented the fall-back of inferring the entire call expression if required.

---

_@abhijeetbodas2001 reviewed on 2025-07-04 15:49_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:205 on 2025-07-04 15:49_

I copied this example here again from above, since there was a different code snippet in between, which was not what the text below intends to refer to.

---

_Comment by @abhijeetbodas2001 on 2025-07-04 16:03_

I've addressed the review comments from above. Major changes are:
- This now restricts these new constraints to be added only at statement-level calls (thanks @sharkdp!), and only in function scopes.
- When required, evaluating the constraints will fall-back to inferring the full call expression. As part of this, the constraint now stores references to both the callable, and the call expression.
- Because we now add these new constraints only inside function scopes, `nox` and `streamlit` no longer panic, [like they used to when we had module-level constraints](https://github.com/astral-sh/ruff/pull/18333#issuecomment-2998963974).
  <details>
  <summary>So I've moved them back to `good.txt`. </summary>
  
  ![image](https://github.com/user-attachments/assets/fa75ec6d-c663-4139-aae4-7dbf398bf5e4)
  </details>

Marking this as ready for another review.



---

_Marked ready for review by @abhijeetbodas2001 on 2025-07-04 16:03_

---

_Review requested from @AlexWaygood by @abhijeetbodas2001 on 2025-07-04 16:03_

---

_Review requested from @dcreager by @abhijeetbodas2001 on 2025-07-04 16:03_

---

_@abhijeetbodas2001 reviewed on 2025-07-04 16:31_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:731 on 2025-07-04 16:31_

I have verified this manually (adding `println!`), but I would also like to add an automated test to verify that this _does not_ infer the whole call expression when not needed. I am not sure how to add such a test though. Any advice on the same would be helpful.

---

_Review requested from @sharkdp by @abhijeetbodas2001 on 2025-07-04 18:07_

---

_Review requested from @carljm by @abhijeetbodas2001 on 2025-07-04 18:07_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:731 on 2025-07-04 18:32_

I think it would be difficult to add such a test and probably not worth it. I expect that benchmarks would let us know if we regress here. (And if they don't, that would suggest this optimization is not actually worth it.)

---

_@carljm approved on 2025-07-04 18:52_

This looks great to me! Really excellent work.

---

_Merged by @carljm on 2025-07-04 18:52_

---

_Closed by @carljm on 2025-07-04 18:52_

---

_Branch deleted on 2025-07-05 06:26_

---
